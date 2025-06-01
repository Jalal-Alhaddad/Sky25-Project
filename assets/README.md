# SKY24 Project

## Useful Links

- [Expo](https://docs.expo.dev/get-started/create-a-project/)
- [Expo Vector Icons](https://docs.expo.dev/guides/icons/)
- [icons.expo.fyi](https://icons.expo.fyi/Index)
- [React Native Paper](https://reactnativepaper.com/)

## 1. Create Expo application

1.1 Create react-native application using:

```bash
npx create-expo-app@latest --template
```

1.2 You maybe asked to download new version of **expo**

![](Images/JH_2024-11-14-11-44-17.png)

1.3 Use **Blank** template [`--template`]

![](Images/JH_2024-11-14-11-44-50.png)

> Name your project "**XX-ROI-Mobile**" XX is your initials

![](Images/JH_2024-11-14-12-11-19.png)

1.4 Install Additional Packages

```bash
npx expo install `
@expo/vector-icons `
react-native-paper `
@react-native-async-storage/async-storage `
react-native-paper-dropdown `
react-native-safe-area-context `
react-native-web `
react-dom `
@expo/metro-runtime `
@react-navigation/native@^6.1.18 `
@react-navigation/bottom-tabs@^6.6.1 `
@react-navigation/stack@^6.4.1 `
react-native-screens
```

1.5 Install `concurrently` package to help run the api with the mobile app

```bash
npm install concurrently --save-dev
```

> Stage and commit "Add new dependencies for navigation and UI components"

## 2. Configure the server

1. Download [server](./assets/server.zip) zip file into the project folder
2. Extract it, so you should find a `server` folder into your project folder
3. This folder contain the api code to be used in your mobile app
4. Update `package.json` by adding the following lines to **scripts**

```json
    ,"seed": "cd server && npm i && npm run seed",
    "start:server": "cd server && npm start",
    "dev": "concurrently \"npm run web\" \"npm run start:server\""
```

![command](Images/JH_2024-11-10-18-48-06.png)

5. Run `npm run seed` to install server' packages and seed the database
6. Run `npm run dev` to test your application, at this moment you have 2 running projects, the server and your app
7. You can use `roiapi.http` from the `server` folder to test the API
8. If encounter upgrade request to the expo SDK, please run the following command:

```bash
npm install expo@latest
```

![](Images/JH_2024-11-14-13-03-16.png)

## 3. Structure your App

3.1 File Naming

- **PascalCase** for screens, components, and navigators
- **lowercase** for folders
- Use **Screen** suffix for screens like `HomeScreen`
- Use **Navigator** suffix for navigators like `RootNavigator`

3.2 Create folders

```bash
|- components
|- navigation
|- screens
|- utils
```

3.3 Create 3 files for your navigators, `Root`, `Main`, `People` (or any name you like `Staff`, `Employees`)

```bash
|- navigation
    |- RootNavigator.js     [Stack Navigator]
    |- MainNavigator.js     [Tab Navigator]
    |- PeopleNavigator.js   [Stack Navigator]
```

3.4 Create Screens file in screens folder for `Home`, `Help`, `Not found`, `New & Edit`, `View`, and `List` records (6 screens)

```bash
|- screens
    |- HomeScreen.js
    |- HelpScreen.js
    |- NotFoundScreen.js
    |- PeopleViewScreen.js
    |- PersonViewScreen.js
    |- PersonEditScreen.js
```

3.5 Create file `api.js` in the `utils` folder for fetching data from the server

```bash
|- utils
    |- api.js
```

## 4. Code Navigators & Basic Screens Layout

- First navigator to be injected in the `App.js` is the `RootNavigator`
- `NavigationContainer` component should surround `RootNavigator` when injected in the `App.js`
- `SafeAreaProvider` should be imported and surround `NavigationContainer`
- `MainNavigator` is first child in `RootNavigator`
- `PeopleNavigator` is the second child in `MainNavigator` where `HomeScreen` is the first, and `HelpScreen` is the last
- Import all required screens and navigators
- Icon name in small letter
- `component` value is the screen file name **without extension**
- `name` attribute is the ID of the screen that can be used in navigation
- Create basic screens layout
- Run the add `npm run dev`

![](Images/JH_2024-11-15-13-44-05.png)

### Map screens to navigators as following:

```bash
|- navigation
    |- RootNavigator.js (Stack Navigator)
      |- MainNavigator.js (Tab Navigator)
          |- HomeScreen.js
          |- PeopleNavigator.js (Stack Navigator)
              |- PeopleViewScreen.js
              |- PersonViewScreen.js
              |- PersonEditScreen.js
          |- HelpScreen.js
      |- NotFoundScreen.js
```

- **RootNavigator = Stack Navigator**
  - Screen name="Main" = MainNavigator
  - Screen name="NotFound" = NotFoundScreen
- **MainNavigator = Tab Navigator**
  - Screen name='Home' = HomeScreen
  - Screen name='People' = PeopleNavigator
  - Screen name='Help' = HelpScreen
- **PeopleNavigator = Stack Navigator**
  - Screen name="PeopleView" = PeopleViewScreen
  - Screen name="PersonView" = PersonViewScreen
  - Screen name="PersonEdit" = PersonEditScreen

> Stage and commit "Add screens and navigators with basic layout"

## 5. Code Data Processing

### Code fetch functions in `utils/api.js` file

- in `api.js` file write code to call asynchronously the api using `fetch` method
  - `export async function fetchDepartments()`
  - `export async function fetchPeople()`
  - `export async function fetchPersonById(id)`
  - `export async function addPerson(personData)`
  - `export async function updatePerson(id, updatedData)`
  - `export async function deletePerson(id)`

### Handle data in the Screens

#### In PeopleViewScreen

- Implement `useState` to store the retrieved data [people] default value ([])
- Implement `useState` to store the offline state [offline] default value (false)
- Implement `useState` to store the error string [error] default value (null)
- Implement `useState` to store the dialog visibility [visible]  default value (false)
- Implement `useState` to store the current selected record id [selectedPersonId] default value (null)
- Implement `useState` to store the current selected record name [selectedPersonName] default value ("")
- Implement `useEffect` to retrieve the data [`fetchPeople`] by calling the api function from the `api.js`
- inside `useEffect` store the retrieved using `setPeople`
- Inside the catch block set the offline and error state
- Use map to list the data like `{people.map(person => (<Text key={person.id}>{person.name}</Text>))}`
- Add delete function
  - `handleDelete()` to call `deletePerson` form `api.js`
- To handle dialog visibility Add:
  - `showDialog(id, name)`
  - `hideDialog()`

### In PersonViewScreen

- Implement `useState` to store the retrieved data [person]  default value (null)
- Implement `useState` to store the offline state [offline] default value (false)
- Implement `useState` to store the error string [error] default value (null)
- Implement `useEffect` to retrieve the data [`fetchPersonById`] by calling the api function from the `api.js`
- inside `useEffect` store the retrieved using `setPerson`
- Inside the catch block set the offline and error state

### In PersonEditScreen

- Implement `useState` to store the retrieved data [person]
- Implement `useState` to store the retrieved data [departments] default value ([])
- Implement `useState` to store the offline state [offline] default value (false)
- Implement `useState` to store the error string [error] default value (null)
- Implement `useEffect` to retrieve the data [`fetchDepartments`,`fetchPersonById`] by calling the api function from the `api.js`
- inside `useEffect` store the retrieved using `setPerson`
- Inside the catch block set the offline and error state
- Add submit function to save or add record
  - `handleSubmit()`

`[person, setPerson] = useState` default value is

```bash
{
    name: "",
    phone: "",
    street: "",
    city: "",
    state: "",
    zip: "",
    country: "",
    departmentId: null,
  }
```

> Stage and commit "Update navigation and screens; refactor dependencies and add API utility functions"

## Implement Navigation Code

### PeopleViewScreen navigation code

- Add show functions that navigate to other screens
  - `showAddPerson()` navigate to `PersonEditScreen` with `id = -1` for new record
  - `showEditPerson(id)` navigate to `PersonEditScreen` with selected record id for update record
  - `showViewPerson(id)` navigate to `PersonViewScreen` with selected record

### PersonViewScreen navigation code

- Add show functions that navigate to other screens
  - `showPeopleView()` to go back to `PeopleViewScreen`

### PersonEditScreen navigation code

- Add show functions that navigate to other screens
  - `showPeopleView()` to go back to `PeopleViewScreen`
