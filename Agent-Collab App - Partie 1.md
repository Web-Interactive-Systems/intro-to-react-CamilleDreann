---
type: NoteCard
createdAt: 2025-05-12T09:31:41.917Z
viewedAt: 2025-05-14T15:23:27.878Z
---

# Agent-Collab App - Partie 1

## Lab1: Création d’une app react avec vite

Créer une app react avec vite : <https://vite.dev/>

## Lab2: Installation de packages

Installer les packages suivants :

```js
npm install @dnd-kit/core@^6.3.1 @dnd-kit/sortable@^10.0.0 @dnd-kit/utilities@^3.2.2 @nanostores/react@^1.0.0 @radix-ui/react-icons@^1.3.2 @radix-ui/themes@^3.2.1 @stitches/react@^1.2.8 marked@^15.0.8 nanostores@^1.0.1 raviger@^4.2.1 re-resizable@^6.11.2
```

## Lab3: Mise en place d’une structure de projet

Créer une structure qui ressemble à ça pour votre app

```js
src
├── App.css
├── App.jsx
├── actions
├── assets
├── components
├── features
├── index.css
├── layouts
├── lib
├── main.jsx
├── pages
├── store
└── utils
```

Dans la racine du projet, ajouter un fichier jsconfig.json avec le suivant

```js
{
  "exclude": ["dist"],
  "compilerOptions": {
    "jsx": "react-jsx",
    "jsxImportSource": "react",
    "baseUrl": "./src",
    "paths": {
      "@/*": ["./*"],
      "@components/*": ["components/*"],
      "@lib/*": ["lib/*"],
      "@data/*": ["data/*"],
      "@actions/*": ["actions/*"],
      "@utils/*": ["utils/*"],
      "@layouts/*": ["layouts/*"],
      "@features/*": ["features/*"]
    }
  }
}
```

## Lab4: Mise en place d’un router

Dans la `layout` créer un `Router.jsx`

```js
import { usePath, useRoutes } from "raviger";
import { useEffect } from "react";

const routes = {
  "/": () => <h1>Hello my home page</h1>,
};

export function Router() {
  const currentPath = usePath();

  useEffect(() => {
    window.scrollTo(0, 0);
  }, [currentPath]);

  const routeResult = useRoutes(routes);

  return routeResult;
}
```

Et dans `App.jsx` ajouter le router

```js
import { Router } from "@/layouts/Router";

function App() {
  return <Router />;
}

export default App;
```

## Lab5.1: Bien comprendre Routing dans SPA

::::Callout{emoji='💡' bgColor='$faint' brColor='$subtle' color='inherent'}
Analyser le comportement du Router. Ajouter d’autres routes pour bien comprendre.
::::

## Lab5.2: Bien comprendre React rendering and useEffect

::::Callout{emoji='💡' bgColor='$faint' brColor='$subtle' color='inherent'}
Ajouter plusieurs console.log dans Router et analyser le comportement de chargement du composant.
::::

## Lab6: Mise en place d’un layout et thème radix

Dans le dossier `components`, créer un composant `Header.jsx`

```js
import { MoonIcon, RocketIcon } from "@radix-ui/react-icons";
import { Button, Flex } from "@radix-ui/themes";
import { Link } from "raviger";

export function Header() {
  return (
    <Flex style={{ boxShadow: "var(--shadow-3)", height: 42, width: "100vw" }}>
      <Flex
        justify="between"
        align="center"
        gap="3"
        width="100%"
        margin="0 auto"
        px="5"
      >
        <Flex gap="6">
          <Link href="/">
            <RocketIcon height="22" width="22" />
          </Link>
        </Flex>

        <Flex justify="center" align="center" direction="row" gap="5">
          <Button variant="ghost" size="4">
            <MoonIcon height="22" width="22" />
          </Button>
        </Flex>
      </Flex>
    </Flex>
  );
}
```

Toujours dans layouts, créer un fichier index.css

```js
body {
  margin: 0;
  min-width: 320px;
  min-height: 100vh;
}

button {
  outline: none !important;
}
```

Toujours dans layouts, créer un composant Theme.jsx

```js
import { Theme as RadixTheme } from "@radix-ui/themes";
import "@radix-ui/themes/styles.css";
import "./index.css";

export default function Theme({ children }) {
  return (
    <RadixTheme
      appearance="light"
      accentColor="indigo"
      scaling="100%"
      radius="full"
    >
      {children}
    </RadixTheme>
  );
}
```

Dans le dossier layouts, créer un composant LayoutTheme.jsx

```js
import { Header } from "@/components/Header";
import { Box, Flex } from "@radix-ui/themes";
import Theme from "./Theme";

export default function LayoutTheme({ children }) {
  return (
    <Theme>
      <Flex direction="column" style={{ minHeight: "100vh" }}>
        <Box>
          <Header />
        </Box>
        <Box width="100%" style={{ height: "calc(100vh - 42px)" }} flex="1">
          {children}
        </Box>
        {/* <Box
          mt='auto'>
          <Footer />
        </Box> */}
      </Flex>
    </Theme>
  );
}
```

Utiliser **LayoutTheme** dans **Router**

```js
import { usePath, useRoutes } from "raviger";
import { useEffect } from "react";
import LayoutTheme from "./LayoutTheme";

const routes = {
  "/": () => <h1>Hello my home page</h1>,
};

export function Router() {
  const currentPath = usePath();

  useEffect(() => {
    window.scrollTo(0, 0);
  }, [currentPath]);

  const routeResult = useRoutes(routes);

  return <LayoutTheme>{routeResult}</LayoutTheme>;
}
```

## Lab7: Bien comprendre React children

::::Callout{emoji='💡' bgColor='$faint' brColor='$subtle' color='inherent'}
Analyser attentivement ce que nous venons de faire dans lab6.

- C’est un composant React
- Comment nous créons des vues avec les composants React
- C’est quoi `children`
  ::::

## Lab8: Intégrer react stitches

Dans le dossier `lib` créer un fichier `stitches.js` et dedans :

```js
import { createStitches } from "@stitches/react";

export const { styled, css, theme, getCssText, keyframes, config, reset } =
  createStitches();
```

Nous allons utliser styled pour customiser les styles des composants

## Lab8.1: Comprendre Stitches

::::Callout{emoji='💡' bgColor='$faint' brColor='$subtle' color='inherent'}
Regarder la documentation de Stitches : <https://stitches.dev/>

A quoi ça sert cette librairie
::::

## Lab9: Création d’un Resizable

Dans le dossier components, créer un composant Resizable.jsx

```js
import * as React from "react";
import { Resizable as ResizablePrimitive } from "re-resizable";
import { styled } from "@/lib/stitches";

const StyledResizable = styled(ResizablePrimitive, {
  //
});

export const Resizable = React.forwardRef(({ children, ...props }, ref) => {
  return (
    <StyledResizable ref={ref} {...props}>
      {children}
    </StyledResizable>
  );
});
```

## Lab10: Création d’une page home

Dans le dossier pages, créer un composant Home.jsx

```js
import { Resizable } from "@/components/Resizable";
import { Flex } from "@radix-ui/themes";

function Home() {
  return (
    <Flex gap="8" width="100%" height="100%">
      <h1>Agent view ici</h1>

      <Resizable
        class="resizable"
        style={{
          background: "var(--focus-a3)",
          borderLeft: "1px solid var(--gray-9)",
          marginLeft: "auto",
        }}
        enable={{
          top: false,
          right: false,
          bottom: false,
          left: true,
          topRight: false,
          bottomRight: false,
          bottomLeft: false,
          topLeft: false,
        }}
      >
        <h1>Chat view ici</h1>
      </Resizable>
    </Flex>
  );
}

export default Home;
```

## Lab11: Bien comprendre js export / import

::::Callout{emoji='💡' bgColor='$faint' brColor='$subtle' color='inherent'}
Analyser la différence entre **export default** versus **export** ainsi que le lien avec l’**import**
::::

## Lab12: Intégration du page home dans le router

Ajouter home dans router

```js
const routes = {
  "/": () => <Home />,
};
```

## Lab13: Création de la vue chat

Dans le dossier features, créer un composant Chat.jsx

```js
import { Flex } from "@radix-ui/themes";
import ChatList from "./ChatList";
import ChatPrompt from "./ChatPrompt";

function Chat() {
  return (
    <Flex direction="column" gap="4" width="100%" height="100%" p="1">
      <ChatList />
      <ChatPrompt />
    </Flex>
  );
}

export default Chat;
```

Dans le dossier features, créer un composant ChatList.jsx

```js
import { Markdown } from "@/components/Markdown";
import { FaceIcon, PersonIcon } from "@radix-ui/react-icons";
import { Box, Flex } from "@radix-ui/themes";
import { useState } from "react";

function ChatList() {
  const [messages, setMessages] = useState([
    { role: "user", content: "bonjour ! comment çava ?", id: "1" },
    {
      role: "assistant",
      content: "bonjour !! je vais bien.. merci.  Je suis là pour aider",
      id: "2",
    },
    { role: "user", content: "aide moi à apprendre react", id: "3" },
  ]);

  return (
    <Flex direction="column" gap="2">
      {messages.map((msg) => (
        <Flex key={`message-${msg.id}`}>
          {msg.role === "assistant" ? (
            <Flex
              style={{
                background: "var(--accent-5)",
                padding: "4px 8px",
                borderRadius: 18,
                marginLeft: 18,
              }}
            >
              <Box>
                <PersonIcon />
              </Box>

              <Markdown content={msg.content || ""} />
            </Flex>
          ) : (
            <Flex
              style={{
                background: "var(--accent-a3)",
                padding: "4px 8px",
                borderRadius: 18,
              }}
            >
              <Box>
                <FaceIcon />
              </Box>
              <Markdown content={msg.content || ""} />
            </Flex>
          )}
        </Flex>
      ))}
    </Flex>
  );
}

export default ChatList;
```

Dans le dossier features, créer un composant ChatPrompt.jsx

```js
import { PaperPlaneIcon } from "@radix-ui/react-icons";
import { Button, Flex, TextArea } from "@radix-ui/themes";
import { useState } from "react";

function ChatPrompt() {
  const [prompt, setPrompt] = useState("");

  const onTextChange = (e) => {
    console.log("onTextChange", e.target.value);
    setPrompt(e.target.value);
  };

  const onSendPrompt = async () => {
    console.log("onSendPrompt", prompt);
  };

  return (
    <Flex justify="center" mt="auto" width="100%">
      <Flex align="center" direction="column">
        <TextArea
          placeholder="Comment puis-je aider..."
          onChange={onTextChange}
        />

        <Flex justify="end" width="100%">
          <Button onClick={onSendPrompt}>
            <PaperPlaneIcon />
          </Button>
        </Flex>
      </Flex>
    </Flex>
  );
}

export default ChatPrompt;
```

## Lab12.1: Améliorer le ChatPrompt

- [ ] Désactiver le bouton d’envoie de message quand le prompt est vide
- [ ] Utiliser Stitches pour améliorer le design du ChatPrompt (voir rendu de la démo)
- [ ] Analyser le rendering du composant suite à la saisie d’un prompt
- [ ] Créer une variable `promptRef` basée sur useRef (<https://react.dev/reference/react/useRef>) pour remplacer la variable prompt (basée sur useState)

## Lab13: Echange de données entre composants

::::Callout{emoji='💡' bgColor='$faint' brColor='$subtle' color='inherent'}
Analyser comment ChatList et ChatPrompt peuvent communiquer, càd, quand je saisie un prompt, il sera ajouté et affiché dans la liste des messages.

L’implémentation ne permet pas cela. Proposer une solution.
::::

## Lab14: Mise en place d’un store

Dans le dossier store, créer un fichier messages.js

```js
import { atom } from "nanostores";

export const $messages = atom([
  { role: "user", content: "bonjour ! comment çava ?", id: "1" },
  {
    role: "assistant",
    content: "bonjour !! je vais bien.. merci.  Je suis là pour aider",
    id: "2",
  },
  { role: "user", content: "aide moi à apprendre react", id: "3" },
]);

export const addMessage = (msg) => {
  // get current messages
  const msgs = $messages.get();
  // add msg to the messages
  $messages.set([...msgs, msg]);
};

export const updateMessages = (msgs) => {
  $messages.set(msgs);
};
```

Dans le dossier store, créer un fichier store.js

```js
export * from "./messages";
```

## Lab15: Utilisation du store dans des composants

Dans le composant ChatList, remplacer la variable d’état messages (basée sur useState) par la variable messages du store

```js
import { useStore } from "@nanostores/react";

// ...
const messages = useStore($messages);
```

Dans le composant ChatPrompt, ajouter un message au store messages (dans la fonction onSendPrompt).

```js
addMessage({
  role: "user",
  content: promptRef.current.value,
  id: Math.random().toString(),
});
```

## Lab15.1: Bien comprendre le store

::::Callout{emoji='💡' bgColor='$faint' brColor='$subtle' color='inherent'}
Regarder la documentation du nanostores : <https://github.com/nanostores/nanostores>

Expliquer son comportement et ses avantages comparé à useState
::::

## Lab16: Mise en place d’un fil de chat

Dans le dossier actions, créer un fichier agent.js avec la fonction onDummyAgent

```js
// function générateur : https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Generator
export const onDummyAgent = async function* () {
  const mockResponses = [
    "Bonne question ! Voici une explication rapide...",
    "Bien sûr ! Explorons cela ensemble.",
    "Voici ce que je peux te dire à ce sujet :",
    "Intéressant ! Voici une réponse possible :",
    "D'accord ! Voici une réponse simulée basée sur ta demande.",
  ];

  // Simuler a retard avant le premier token
  await new Promise((resolve) =>
    setTimeout(resolve, Math.random() * 1000 + 500)
  );

  // Sélectionner une réponse random
  const response =
    mockResponses[Math.floor(Math.random() * mockResponses.length)];

  // Stream la réponse caractères par caractères avec un petit retard
  for (let i = 0; i < response.length; i++) {
    yield response[i];
    await new Promise((resolve) =>
      setTimeout(resolve, 30 + Math.random() * 50)
    ); // simulate typing
  }
};
```

**Todo :**

- [ ] Utiliser la fonction onDummyAgent pour créer un fil de chat avec un stream de la réponse d’IA

```js
// appeler l'agent IA
    for await (const token of onDummyAgent()) {
      console.log('token', token, last)
     // ...
      updateMessages([...])
    }
```
