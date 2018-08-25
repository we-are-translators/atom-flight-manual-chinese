# 包：活动编辑器信息

在 [字数统计](/linux/chapter3/package-word-count) 包中我们了解了如何在模态面板中展示信息。不过，面板并不是扩展 Atom UI 的唯一方式 — 你也可以添加项目到工作区。这些项目可以被拖到新的区域（比如，在窗口边上的一个停靠项），Atom 将在你下次打开项目时恢复它们。Atom 的树形界面以及第三方包像 [Nuclide](https://nuclide.io/) 的控制台，调试，轮廓界面，以及诊断（linter  结果）都使用了该系统。

对于此包，我们将定义一个工作区域项目，告诉我们一些关于我们的活动文本编辑器的信息。最终实现的包可以在 [https://github.com/atom/active-editor-info](https://github.com/atom/active-editor-info) 查看。


## 创建包

首先，按 <kbd>Cmd+Shift+P</kbd> 打开 [指令面板](https://github.com/atom/command-palette)。输入 "generate package" 并选择 "Package Generator: Generator Package" 指令，就像我们在 [这个章节的包生成器](/linux/chapter3/package-word-count?id=包生成器) 小节中做的那样。输入 `active-editor-info` 作为包名。

## 添加窗口

现在让我们编辑包文件以便在工作区中显示我们的视图界面而不是在模态框中。我们做这件事的方式是通过用 Atom 注册一个 `opener`。窗口仅仅是接收一个 URI 并返回一个视图界面（如果它是该窗口知道的 URI）的函数。当你调用 `atom.workspace.open()` 时，Atom 将遍历所有的窗口直到它找到一个能处理你传入的 URI 。

让我们打开 `lib/active-editor-info.js` 并编辑 `activate()` 方法来注册一个窗口：

```js
'use babel';

import ActiveEditorInfoView from './active-editor-info-view';
import {CompositeDisposable, Disposable} from 'atom';

export default {

  subscriptions: null,

  activate(state) {
    this.subscriptions = new CompositeDisposable(
      // Add an opener for our view.
      atom.workspace.addOpener(uri => {
        if (uri === 'atom://active-editor-info') {
          return new ActiveEditorInfoView();
        }
      }),

      // Register command that toggles this view
      atom.commands.add('atom-workspace', {
        'active-editor-info:toggle': () => this.toggle()
      }),

      // Destroy any ActiveEditorInfoViews when the package is deactivated.
      new Disposable(() => {
        atom.workspace.getPaneItems().forEach(item => {
          if (item instanceof ActiveEditorInfoView) {
            item.destroy();
          }
        });
      })
    );
  },

  deactivate() {
    this.subscriptions.dispose();
  },

  toggle() {
    console.log('Toggle it!')
  }

};
```

你会发现我们也移除了 `activeEditorInfoView` 属性和 `serialize()` 方法。那是因为，使用工作区可以有多个给定视图界面的实例。由于每个实例有它们自己的状态，所以每一个实例应该做它自己的序列化而不是依赖包级的 `serialize()` 方法。我们后面再回来详细了解包级序列化。

你也可能发现我们的 `toggle()` 实现只是输入日志文本 "Toggle it!" 到控制台。让我们使它能实际切换我们的视图界面：

```js
toggle() {
  atom.workspace.toggle('atom://active-editor-info');
}
```

## 更新视图界面

Atom 在任何地方都使用相同的抽象视图，所以我们几乎可以使用现有生成的 ActiveEditorInfoView 类。我们只需要添加两个小方法：

```js
getTitle() {
  // Used by Atom for tab text
  return 'Active Editor Info';
}

getURI() {
  // Used by Atom to identify the view when toggling.
  return 'atom://active-editor-info';
}
```

现在重启窗口并从指令面板运行 "Active Editor Info: Toggle" 指令！我们的界面将出现在工作区中心的一个新的标签页中。如果你想要的话，你可以拖拽它到任何一个可以停靠的位置。再次切换将隐藏那个停靠的位置。如果你关闭该标签页并重新运行切换指令，它将出现在你最后放置它的位置。

> 我们现在已经重复了相同的 URI 三次了。没关系，不过把 URL 定义在一个地方然后在你需要的地方从那个模块中导入可能是一个好的方法。

## 限制我们项目的位置

我们的界面的目的是显示关于活动文本编辑器的信息，所以在工作区的中心位置（那是文本编辑器的位置）显示我们的项目真的没有什么意义。让我们添加一些方法给我们的视图类来控制打开的位置：

```js
getDefaultLocation() {
  // This location will be used if the user hasn't overridden it by dragging the item elsewhere.
  // Valid values are "left", "right", "bottom", and "center" (the default).
  return 'right';
}

getAllowedLocations() {
  // The locations into which the item can be moved.
  return ['left', 'right', 'bottom'];
}
```

现在我们的项目将初始化显示在右边的停靠位置，用户也只能拖拽它到其它的某个停靠位置。

## 显示活动编辑器信息

现在我们的界面已经都串联起来了，让我们更新它来显示一些关于活动文本编辑器的信息。添加这些代码到构造函数中：

```js
this.subscriptions = atom.workspace.getCenter().observeActivePaneItem(item => {
  if (!atom.workspace.isTextEditor(item)) {
    message.innerText = 'Open a file to see important information about it.';
    return;
  }
  message.innerHTML = `
    <h2>${item.getFileName() || 'untitled'}</h2>
    <ul>
      <li><b>Soft Wrap:</b> ${item.softWrapped}</li>
      <li><b>Tab Length:</b> ${item.getTabLength()}</li>
      <li><b>Encoding:</b> ${item.getEncoding()}</li>
      <li><b>Line Count:</b> ${item.getLineCount()}</li>
    </ul>
  `;
});
```

现在只要你在中心位置打开一个文本编辑器，该界面将更新一些关于它的信息。

> 我们在这里使用模版字符串，因为它很简单，而且我们可以有效的控制其行为，不过如果你不注意的话很容易导致插入不希望的 HTML 。实际操作中你需要净化输入并使用 DOM API 或者使用模版系统。

也不要忘了在 `destory()` 方法中清理订阅：

```js
destroy() {
  this.element.remove();
  this.subscriptions.dispose();
}
```

## 序列化

如果你现在重启了 Atom ，你会看到我们的项目已经消失了。那是因为我们还没有告诉 Atom 如何序列化它。现在，让我们来做这件事吧。

第一步是在我们的 ActiveEditorInfoView 上实现一个 `serialize()` 方法。Atom 将在工作区的每个项目上周期性调用 `serialize()` 方法来保存其状态。

```js
serialize() {
  return {
    // This is used to look up the deserializer function. It can be any string, but it needs to be
    // unique across all packages!
    deserializer: 'active-editor-info/ActiveEditorInfoView'
  };
}
```

我们所有的视图状态都来自于活动文本编辑器，所以我们只需要 `deserializer` 字段。如果我们有其它想要重启保存的状态，我们只要添加字段到我们返回的对象里。只要确保他们是 JSON 序列化的！

下一步我们需要注册一个 Atom 启动时可以用于重新创建真实对象的反序列化函数。最好的方式是添加一个 "deserializers" 对象到 `package.json` 文件：

```json
{
  "name": "active-editor-info",
  ...
  "deserializers": {
    "active-editor-info/ActiveEditorInfoView": "deserializeActiveEditorInfoView"
  }
}
```

注意关键字（"active-editor-info/ActiveEditorInfoView"）匹配我们在上面使用的 `serialize()` 方法中的字符串。值（"deserializeActiveEditorInfoView"）引用在我们主模块中的一个函数，我们依旧需要添加这个函数。回到 `active-editor-info.js` 并添加该函数：

```js
deserializeActiveEditorInfoView(serialized) {
  return new ActiveEditorInfoView();
}
```

从我们的 `serialize()` 方法返回的值将传给该函数。由于我们的序列化对象不包含任何状态，我们只能返回一个新的 ActiveEditorInfoView 实例。

重启 Atom 并用 "Active Editor Info: Toggle" 切换视图界面。然后再次重启 Atom 。你的试图界面应该是刚才离开的地方！

## 小结

在本章节中，我们已经制作了一个可切换的工作区项目，它可以通过用户控制它放置的位置。当创建用于代码工作的各种各样的可视化工具是这是非常有用的！
