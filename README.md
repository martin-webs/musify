# Build a Full-stack App from Scratch

## SET UP
First, we set up our configuration; package.json should look like this:

```bash
{
  "name": "myapp",
  "private": true,
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start",
    "lint": "next lint"
  },
  "dependencies": {
    "@chakra-ui/layout": "^1.5.1",
    "@chakra-ui/react": "^1.7.2",
    "@dh-react-hooks/use-raf": "^0.9.1",
    "@emotion/react": "^11.6.0",
    "@emotion/styled": "^11.6.0",
    "@prisma/client": "^3.6.0",
    "bcrypt": "^5.0.1",
    "cookie": "^0.4.1",
    "cookies": "^0.8.0",
    "easy-peasy": "^5.0.4",
    "format-duration": "^1.4.0",
    "framer-motion": "^4.1.17",
    "jsonwebtoken": "^8.5.1",
    "next": "12.0.7",
    "react": "17.0.2",
    "react-dom": "17.0.2",
    "react-howler": "^5.2.0",
    "react-icons": "^4.3.1",
    "reset-css": "^5.0.1",
    "swr": "^1.1.1"
  },
  "devDependencies": {
    "@types/react": "^17.0.37",
    "@typescript-eslint/eslint-plugin": "^5.6.0",
    "@typescript-eslint/parser": "^5.6.0",
    "babel-plugin-superjson-next": "^0.4.2",
    "eslint": "^8.4.1",
    "eslint-config-airbnb": "^19.0.2",
    "eslint-config-next": "12.0.7",
    "eslint-config-prettier": "^8.3.0",
    "eslint-plugin-import": "^2.25.3",
    "eslint-plugin-jsx-a11y": "^6.5.1",
    "eslint-plugin-prettier": "^4.0.0",
    "eslint-plugin-react": "^7.27.1",
    "eslint-plugin-react-hooks": "^4.3.0",
    "prettier": "^2.4.1",
    "prisma": "^3.6.0",
    "ts-node": "^10.4.0"
  },
  "prisma": {
    "seed": "ts-node --compiler-options {\"module\":\"CommonJS\"} prisma/seed.ts"
  }
}
```

One of the dependencies does not work with React 17 or older, (`@dh-react-hooks/use-raf`), so I am removing it, otherwise npm does not let me install all the other dependencies. I will check other options, here is one from [rooks](https://react-hooks.org/docs/useRaf) (TBC).

Add these too:

.bablerc
```bash
{
  "presets": ["next/babel"],
  "plugins": ["superjson-next"]}
```
.eslintrc.js
```bash
module.exports = {
  extends: ['next/core-web-vitals', 'airbnb', 'airbnb/hooks', 'prettier'],
  plugins: ['react', '@typescript-eslint', 'prettier'],
  env: {
    browser: true,
    es2021: true,
    node: true,
  },
  parser: '@typescript-eslint/parser',
  parserOptions: {
    ecmaFeatures: {
      jsx: true,
    },
    ecmaVersion: 13,
    sourceType: 'module',
  },
  rules: {
    'react/react-in-jsx-scope': 'off',
    'react/prop-types': 'off',
    'react/jsx-props-no-spreading': 'off',
    'import/prefer-default-export': 'off',
    'no-param-reassign': 'off',
    'import/extensions': [
      'error',
      'ignorePackages',
      {
        ts: 'never',
        tsx: 'never',
      },
    ],
    'consistent-return': 'off',
    'arrow-body-style': 'off',
    'prefer-arrow-callback': 'off',
    'react/jsx-filename-extension': 'off',
    'react/function-component-definition': [
      'error',
      {
        namedComponents: 'arrow-function',
        unnamedComponents: 'arrow-function',
      },
    ],
    'prettier/prettier': 'warn',
  },
}
```
.gitignore
```bash
# See https://help.github.com/articles/ignoring-files/ for more about ignoring files.

# dependencies
/node_modules
/.pnp
.pnp.js

# testing
/coverage

# next.js
/.next/
/out/

# production
/build

# misc
.DS_Store
*.pem

# debug
npm-debug.log*
yarn-debug.log*
yarn-error.log*

# local env files
.env.local
.env.development.local
.env.test.local
.env.production.local
.env

# vercel
.vercel
```
.prettierrc.js
```bash
module.exports = {
  trailingComma: "es5",
  tabWidth: 2,
  semi: false,
  singleQuote: true,
}
```
tsconfig.json
```bash
{
  "compilerOptions": {
    "target": "es5",
    "lib": [
      "dom",
      "dom.iterable",
      "esnext"
    ],
    "allowJs": true,
    "skipLibCheck": true,
    "strict": false,
    "downlevelIteration": true,
    "forceConsistentCasingInFileNames": true,
    "noEmit": true,
    "incremental": true,
    "esModuleInterop": true,
    "module": "esnext",
    "moduleResolution": "node",
    "resolveJsonModule": true,
    "isolatedModules": true,
    "jsx": "preserve"
  },
  "include": [
    "next-env.d.ts",
    "**/*.ts",
    "**/*.tsx"
  ],
  "exclude": [
    "node_modules"
  ]
}
```

You can find the repo [here](https://github.com/Hendrixer/fullstack-music).

So, you should run these to get the project started:
```bash
npx create-next-app myapp
```
Then, install all the dependencies above, either by hand, individually, or just paste them in the package.json, using this command:
```bash
npm install 
```

## APP LAYOUT
We will use Chakra UI, which is a library that provides ready-made components with the ability to use custom the styling.

First, we import the `provider` that Chakra brings, evertyhing that is in this component will have access to certain properties; `extendTheme` will give us more options for the layout; `reset-css` will make our styling consistent accross browsers (already installed with package.json provided).
```bash
import { ChakraProvider, extendTheme } from "@chakra-ui/react";
import "reset-css"
```
We make changes to our layout using extendTheme:
```bash
const theme = extendTheme({
  colors: {
    100: "#f5f5f5",
    200: "#EEEEEE",
    300: "#E0E0E0",
    400: "#BDBDBD",
    500: "#9E9E9E",
    600: "#757575",
    700: "#616161",
    800: "#424242",
    900: "#212121",
  },
  components: {
    Button: {
      variants: {
        link: {
          ":focus": {
            outline: "none",
            boxShadow: "none",
          },
        },
      },
    },
  },
});
```
We have also customized the Button component; we got rid of the outline and box-shadow that come with Chakra by default.

We'll wrap our app with ChakraProvider: 
```bash
<ChakraProvider theme={theme}>
	<Component {...pageProps} />
</ChakraProvider>
```
Next we create a component named `PlayerLayout` (we create a `components` folder in the root of our project and place `PlayerLayout` there. We call the file `playerLayout.tsx` (or better `PlayerLayout.tsx?`). BTW, tsx means `typescript jsx`). 

`PlayerLayout` will return a `<Box>`, which is a `<div>` in the Chackra UI world, containing `{children}`, which is whatever we place inside the `PlayerLayout` component in `_app.tsx` (in this case the homepage we currently have, just as an example).

Technically, we could have wrap `PlayerLayout` around each component instead, but we would not notice it. But in this case, we prefer not to; our app will have a player in the layout, that player would reload for any change happening in other components and we do not want that. You can wrap `PlayerLayout` in other components if its content is static though.

However this creates a problem: a sign up/in page would have to live inside this global layout, too and we don't want that either. We'll get around this problem now.

## Styling PlayLayout
`<Box>` is useful since we can pass CSS properties as props, right out of the box(they literally mapped every CSS property to be passed as a prop in the `<Box>` component). This saves us the need to use CSS preprocessors, CSS lives within the component.

The recommendation is, use fixed sizes for the navigation bar, "navigation is one of those things that should never change its dimensions, no matter what screen size is." So we are using absolute values:

```bash
const PlayerLayout = ({ children }) => {
  return (
    <Box width="100vw" height="100vh">
      <Box position="absolute" top="0" width="250px" left="0">
        sidebar
      </Box>
      <Box marginLeft="250px" marginBottom="100px">
        {children}
      </Box>
      <Box position="absolute" left="0" bottom="0">
        Player
      </Box>
    </Box>
  );
};
```

Now we clean `index.tsx`, we just add the string `Home` for now.

We create another component, `sidebar.tsx`, import it to `playerLayout.tsx` and check it if works. It should.

```bash
    <Box width="100vw" height="100vh">
      <Box position="absolute" top="0" width="250px" left="0">
        <Sidebar />
      </Box>
      <Box marginLeft="250px" marginBottom="100px">
        {children}
      </Box>
      <Box position="absolute" left="0" bottom="0">
        Player
      </Box>
    </Box>
```

## STYLING THE SIDEBAR
Basically, we will use chakra-ui and react-icons, icons from Material Design, to mock the Spotify layout and icons.

```bash
import {
  Box,
  List,
  ListItem,
  ListIcon,
  Divider,
  Center,
  LinkBox,
	LinkOverlay
} from "@chakra-ui/layout";
import {
	MdHome,
	MdSearch,
	MdLibraryMusic,
	MdPlaylistAdd,
	MdFavorite
} from 'react-icons/md'

const Sidebar = () => {
  return <div>sidebar</div>;
};
export default Sidebar;
```
Now that the layout is set, we add a <`Box`> container to manage the padding in the Y axis, inside we start adding the content: we make a <`Box`> to manage `<NextImage>`, imported from 'next/image', which makes you images web-ready.
The next <`Box`> will have bottom-margin="20px" to account for the space between the two submenus under the logo.

We use `<NextLink>` within `<LinkBox>` to prevent the app from making requests to the server when the user clicks on a link, instead, the request will be made in the client-side. Within `<NextLink>` we add the `passHref` property, so the href is passed to its child component, in this case `<LinkOverlay>`.

```bash
     <Box paddingY="20px">
				<Box width="120px" marginBottom="20px" paddingX="20px">
					<NextImage src="/logo.svg" height={60} width={120}/>
				</Box>
				<Box marginBottom="20px">
					<List spacing={2}>
						{navMenu.map(menu => (
							<ListItem paddingX="20px" fontSize="16px" key={menu.name}>
								<LinkBox>
									<NextLink href={menu.route} passHref>
										<LinkOverlay>
											<ListIcon as={menu.icon} color="white" marginRight="20px"/>
											{menu.name}
										</LinkOverlay>
									</NextLink>
								</LinkBox>
							</ListItem>
						))}
					</List>
				</Box>
			</Box>
    </Box>
```


