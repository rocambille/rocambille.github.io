---
ref: 2022-05-02-how-to-do-a-modal-in-react-the-html-first-approach
lang: fr
title: 'Comment faire une modale avec React : l''approche "HTML first"'
tags: html react javascript
authors:
  - romain-guillemot
---

![montrez moi le chemin](/assets/2022-05-02-how-to-do-a-modal-in-react-the-html-first-approach-cover-min.jpeg)

Faites du HTML avant de faire du CSS, ou du JS... ou du React.<!--more-->

## Au commencement était une modale

Cette histoire a commencé avec une modale. J'avais besoin d'une fenêtre modale dans un projet React. Pour rappel, voici une bonne définition de [wikipedia](https://en.wikipedia.org/wiki/Modal_window):

> Une fenêtre modale crée un mode qui désactive la fenêtre principale mais la garde visible, avec la fenêtre modale comme fenêtre enfant devant elle. Les utilisateurs _doivent_ interagir avec la fenêtre modale avant de pouvoir revenir à l'application parente.

En utilisant React, cela peut prendre la forme suivante :

```react
<Modal trigger={<button type="button">Cliquez moi</button>}>
  Lorem ipsum dans une modale
</Modal>
```

Avec une première implémentation du composant `Modal` :

```react
function Modal ({ trigger, children }) {
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
            X
          </button>
          <div>{children}</div>
        </div>
      )}
    </>
  );
}
```

J'ai supprimé les noms de classe et le style pour me concentrer sur la logique de la modale et sa sémantique. C'est un premier problème ici : la **sémantique**.

La modale est composé d'un déclencheur (_trigger_) et d'un contenu (_children_). Sauf que le contenu n'est pas explicitement décrit comme un contenu de fenêtre modale. De plus, ce composant `Modal` gère le déclencheur et le contenu via différents mécanismes :

- Le trigger est une prop, en attente d'un élément (un conteneur et un contenu : ici un `<button>` avec un texte "Cliquez moi").
- Le _lorem ipsum_ est le contenu du composant, passé comme nœud de rendu (contenu uniquement : le composant `Modal` enveloppera le texte dans une `<div>`).

## Puis vinrent les sous-composants

Une version plus sémantique et cohérente pourrait être :

```react
<Modale>
  <Modal.Trigger>Cliquez-moi</Modal.Trigger>
  <Modal.Window>
    Lorem ipsum dans une modale
  </Modal.Window>
</Modal>
```

Ici le déclencheur et la fenêtre sont au même niveau, tandis que le _lorem ipsum_ est explicitement le contenu de la fenêtre modale. Cela peut être réalisé en déclarant des nouveaux composants `Trigger` et `Window` en tant que propriétés de `Modal`. Ce sont des sous-composants de React. Quelque chose comme ça :

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

Selon notre implémentation précédente, `Trigger` et `Window` devraient afficher les boutons d'ouverture/fermeture. `Modal` est un conteneur et se contentera d'afficher ses enfants :

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

  retour isOpen && (
    <div>
      <button
        type="button"
        onClick={() => setOpen(false)}>
        X
      </button>
      {children}
    </div>
  );
}

Modal.Window = Window;
```

Sauf que `isOpen` et `setOpen` font partie de l'état de `Modal`. Ils doivent donc être passés à ses enfants. Un _prop drilling_ complexe. Complexe car il faudra d'abord "parser" les enfants pour récupérer `Trigger` et `Window`... Prenons la solution de facilité avec l'API Context :

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

  retour isOpen && (
    <div>
      <button
        type="button"
        onClick={() => setOpen(false)}>
        X
      </button>
      {children}
    </div>
  );
}

Modal.Window = Window;
```

Quelle beauté ! N'est-il pas ?

## L'approche HTML first

C'est une beauté. Vraiment. Une telle beauté qu'elle a été ajoutée à HTML depuis des années. Un élément avec un état ouvert/fermé, déclenché par un enfant, et contrôlant l'affichage de son contenu. Voilà les balises `<details>` et `<summary>`. Elles permettent à notre `Modal` de prendre une nouvelle forme :

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

Une démo complète avec un peu de style est disponible ici : [https://codepen.io/rocambille/pen/poaoKYm](https://codepen.io/rocambille/pen/poaoKYm).

Parfois, nous voulons développer des idées. Et parfois, nous les voulons si fort que nous commençons à écrire du code. Utiliser du JS ou tout autre langage/outil/framework, car c'est ce que nous avons appris. Utiliser du CSS pur lorsque cela est possible.

Parfois, nous devrions faire du HTML avant de faire du CSS, ou du JS... ou du React. En utilisant une approche _HTML first_ ;)
