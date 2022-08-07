---
ref: how-to-do-a-modal-in-react-the-html-first-approach
lang: fr
layout: post
title: 'Comment faire une modale avec React : l''approche "HTML first"'
tags: javascript react html
---

![show me the way](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/m18m2ve02cet84x2us9g.jpg)

Do HTML before doing CSS, or JS... or React.

## First, there was a modal

This story started with a modal. I needed a modal window in a React project. As a recall, here is a good definition from [wikipedia](https://en.wikipedia.org/wiki/Modal_window):

> A modal window creates a mode that disables the main window but keeps it visible, with the modal window as a child window in front of it. Users _must_ interact with the modal window before they can return to the parent application.

Using React, this can take the form:

```react
<Modal trigger={<button type="button">Click me</button>}>
  Lorem ipsum in a modal
</Modal>
```

With a first implementation of the `Modal` component:

```react
function Modal({ trigger, children }) {
  const [isOpen, setOpen] = useState(false);

  return (
    <>
      {React.cloneElement(trigger, {
        onClick: () => setOpen(true)
      })}
      {isOpen && (
        <div>
          <button
            type="button"
            onClick={() => setOpen(false)}>
            x
          </button>
          <div>{children}</div>
        </div>
      )}
    </>
  );
}
```

I removed the class names and the style to focus on the modal logic and semantic. That's a first issue here: the **semantic**.

The modal is composed with the trigger and the content of the modal window. Except the content isn't qualified as a "modal window" content. Moreover this `Modal` handles the trigger and the content through different mechanisms:

- The trigger is a prop, waiting for an element (container + content: here a `<button>` with a "Click me" text).
- The _lorem ipsum_ is the content of the component, passed as a rendering node (content only: the `Modal` wraps the text in a `<div>`).

## And then, there were the subcomponents

A more semantic, consistent version could be:

```react
<Modal>
  <Modal.Trigger>Click me</Modal.Trigger>
  <Modal.Window>
    Lorem ipsum in a modal
  </Modal.Window>
</Modal>
```

Here the trigger and the window are in the same level, while the _lorem ipsum_ is qualified as the modal window content. In a nutshell, this can be achieved by declaring new components `Trigger` and `Window` as properties of `Modal`. These are React subcomponents. Something like that:

```react
function Modal(/* ... */) {
  /* ... */
}

function Trigger(/* ... */) {
  /* ... */
}

Modal.Trigger = Trigger;

function Window(/* ... */) {
  /* ... */
}

Modal.Window = Window;
```

Following our previous implementation, `Trigger` and `Window` should display the open/close buttons. Modal is a container, and should display its children:

```react
function Modal({ children }) {
  const [isOpen, setOpen] = useState(false);

  return (
    <>
      {children}
    </>
  );
}

function Trigger({ children }) {
  /* ... */

  return (
    <button
      type="button"
      onClick={() => setOpen(true)}>
      {children}
    </button>
  );
}

Modal.Trigger = Trigger;

function Window({ children }) {
  /* ... */

  return isOpen && (
    <div>
      <button
        type="button"
        onClick={() => setOpen(false)}>
        x
      </button>
      {children}
    </div>
  );
}

Modal.Window = Window;
```

Except `isOpen` and `setOpen` are parts of the modal state. So they must be passed to the modal children. A complex prop drilling. Complex because first you will have to "parse" the children to retrieve `Trigger` and `Window`... Let's take the easy way out with the Context API:

```react
const ModalContext = createContext();

function Modal({ children }) {
  const [isOpen, setOpen] = useState(false);

  return (
    <ModalContext.Provider value={ { isOpen, setOpen } }>
      {children}
    </ModalContext.Provider>
  );
}

function Trigger({ children }) {
  const { setOpen } = useContext(ModalContext);

  return (
    <button
      type="button"
      onClick={() => setOpen(true)}>
      {children}
    </button>
  );
}

Modal.Trigger = Trigger;

function Window({ children }) {
  const { isOpen, setOpen } = useContext(ModalContext);

  return isOpen && (
    <div>
      <button
        type="button"
        onClick={() => setOpen(false)}>
        x
      </button>
      {children}
    </div>
  );
}

Modal.Window = Window;
```

What a beauty! Or is it really?

## The HTML first approach

It was. Really. Such a beauty this was added to HTML ages ago. An element with an open/close state, triggered by a child, and controlling the display of its content. There are the `<details>` and `<summary>` tags. They make our `Modal` become:

```react
function Modal({ children }) {
  return <details>{children}</details>;
}

function Trigger({ children }) {
  return <summary>{children}</summary>;
}

Modal.Trigger = Trigger;

function Window({ children }) {
  return <div>{children}</div>;
}

Modal.Window = Window;
```

A complete demo with some style is available here: https://codepen.io/rocambille/pen/poaoKYm.

Sometimes, we want things. And sometimes, we want them so hard we start writing code. Using JS or any other language/tool/framework, because that's what we learned. Using pure CSS when possible.

Sometimes we should do HTML before doing CSS, or JS... or React. Using an _HTML first_ approach ;)
