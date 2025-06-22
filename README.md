# E-Plant Shopping

## Task 1: ProductList Component Layout

The product page will allow your users to shop for the different plants you sell. Each plant will display on its own "card" with its related data stored in the plant object. You will store the plant objects in an array. Follow these steps for the array and plant objects.

### Display the Plant Array
- Navigate to the ProductList.jsx component and you will see an array named plantsArray with the plants details.
- Each plant object contains the categories, properties name, image URL, description, and cost.
- Display Plant Details within div tag with class name product-grid.
- Utilize array methods to map over the plant array.

**Hint:** use the map() method to iterate array.

- Render each plant's details on the page, including name, image, description, and cost.
- Display an Add to Cart button for each plant.
- Include above code within class name product-grid.
- Create one variable named addedToCart for state management using the useState hook to track which products are added to the cart.

### Add to Cart Functionality
Create the handleAddToCart function to implement the functionality for adding a plant to the cart when the user selects the Add to Cart button. This function should take one parameter that contains the information of the selected plant. This information should then be dispatched to the addItem inside the function component CartSlice.

Additionally, reflect the product has been added to the cart. Update the setAddedToCart state to by setting the product name as a key and its value to true.

**Note:** Make sure that you import the addItem reducer from CartSlice.jsx

The handleAddToCart() function will carry the details of that plant which user want to add in the cart. And the plant details to the cart at a global level using CartSlice.jsx.

Make sure that you save these changes by pushing your code to your GitHub repository.

## Task 2: State Management using Redux

You have the basic layout in the CartSlice.jsx file.

### Define Reducer Functions

Now implement the reducer property of the slice for adding, removing, and updating the number of items in the cart.

These reducer functions will be called when user wants to add or remove the quantity of plants within the cartItems component.

The addItem() reducer adds a new plant item to the items array which you initialized in the previous step.

The addItem() function should get called when the user selects an Add to cart on the plant listing page. Subsequently, the handleAddToCart() gets called which has the plant type as a parameter.

The handleAddToCart() function will then dispatch the plant details to the addItem() reducer function in CartSlice.jsx.

Now you need to complete code for the removeItem() and updateQuantity() reducers.

**removeItem()**: This reducer removes an item from the cart based on its name and gets called when the user wants to remove products from the cart.

**updateQuantity()**: To create this function, start by extracting the item's name and amount from the action.payload. Then, look for the item in the state.items array that matches the extracted name. If the item is found, update its quantity to the new amount provided in the payload. This ensures the item's quantity is correctly updated based on the action.

### Handle Actions

Export the action creators, addItem() to use in ProductList.jsx, removeItem(), and updateQuantity(), to use in the CartItem.jsx.
Also export the reducer as the default to use in store.js.

## Task 3: CartItems Component

Next, you will complete the development of the CartItem.jsx component which displays the items held in the shopping cart. This component has a number of functionalities that you find in a typical shopping cart:

### Cost of All Items in Cart

In the calculateTotalAmount() you need a function to calculate the cost of all of the items in the cart. There are a number of ways you can calculate this.

**Sample algorithm description for calculating the total cost:**
- Initialize a variable total to hold the cumulative sum.
- Iterate over the cart array using cart.forEach().
- For each item, extract its quantity and cost.
- Convert the cost string (e.g., "$10.00") to a number using parseFloat(item.cost.substring(1)), then multiply it by the quantity.
- Add the resulting value to total.
- After processing all items, return the final total sum.

### Continue Shopping

Users should be able to return to the plant listing page to continue shopping while on the shopping cart page. So, in the handleContinueShopping() function, call the onContinueShopping(e) function passed from the parent component.

### Checkout

In this project, you are not required to provide the handleCheckoutShopping() function, but you may wish to for further exploration and practice. For now, just add in the following code to alert the user this function will get added at a later date.

```javascript
const handleCheckoutShopping = (e) => {
  alert('Functionality to be added for future reference');
};
```

### Increment and Decrement

For the handleIncrement() and handleDecrement() functions, you need to dispatch the updateQuantity() reducer in the CartSlice.jsx file. In the function argument, either add one to the item.quantity value or subtract one, respectively.

Also, for the handleDecrement() you will need an if-else to handle the case:
- If the item's quantity is greater than 1, dispatch updateQuantity to decrease the quantity by 1.
- Else if the quantity would drop to 0, dispatch the removeItem action to remove the plant type from the cart.

**Example code:**
```javascript
dispatch(updateQuantity({ name: item.name, quantity: item.quantity + 1 }));
```

### Remove Plant from the Cart

For the handleRemove() function you need to dispatch the removeItem action to delete the item from the cart.

### Item Subtotal

Calculate the total cost for an item by multiplying its quantity with its unit price in the calculateTotalCost() function.
Extract the numeric value from the item's cost string using parseFloat(item.cost.substring(1)) before performing the multiplication.

**Note:** Ensure that these event handlers update the UI in real time. When the user changes the number of a plant type, the following should update accordingly:
- The individual plant quantity
- The item's subtotal
- The overall total cost
- The total number of items in the cart icon
- After a user clicks the "Add to Cart" button for a plant item, the button should become disabled and grayed out, and its label should update to "Added to Cart" to indicate that the item has already been added

### Key Functionalities:
- Calculate the total for all items in the cart
- Calculate the subtotal for each plant type in the cart
- Continue shopping
- Increment and decrement the number of each plant type in the cart
- Remove (delete) a plant type from the cart altogether
- You will dispatch the increment, decrement, and update quantity details from a Redux file

## Task 4: Integrate Redux Functionality in Your Components

### ProductList Component

Initialize the cart state within the Redux store to keep track of cart items.

**Hint:**
```javascript
dispatch(addItem(product));
```

- Use the addItem action to add selected products to the cart.
- Access the Redux store to retrieve and display the total quantity of items currently in the cart.

**Hint:**
```javascript
const calculateTotalQuantity = () => {
  return CartItems ? CartItems.reduce((total, item) => total + item.quantity, 0) : 0;
};
```

### CartItem Component

- Use the updateQuantity action to change how many items are in the cart.
- Use the addItem action to add a new product to the cart.
- Use the removeItem action to delete an item completely from the cart.

**Hint:**
```javascript
dispatch(removeItem(item.name));
```

## Task 5: Import Details to store.js

### Importing Necessary Functions and Files:

The configureStore() function from the @reduxjs/toolkit package is imported to set up the Redux store.
The cartReducer from the CartSlice.jsx file which is imported, manages the state slice related to the shopping cart.

```javascript
import { configureStore } from '@reduxjs/toolkit';
import cartReducer from './CartSlice';
```

### Configuring the Store:

The configureStore() function is used to setup the Redux store.
Inside the configuration object passed to configureStore(), the reducer key specifies the reducer functions. In this case, the cartReducer is assigned to manage the cart slice of the state.

```javascript
// Create a Redux store using configureStore from Redux Toolkit
const store = configureStore({
    // Define the root reducer object
    reducer: {
        // 'cart' is the name of the slice in the store, and it's managed by cartReducer
        cart: cartReducer,
    },
});
export default store; // Export the store for use in the app (e.g., in <Provider store={store}>)
```

### Exporting the Store:

The configured Redux store is exported using export default store;, so it can be used throughout the application to manage state.

```javascript
export default store;
```

**Note:**
- This code is pre-configured in the repository and ready for use.
- Make sure that you save these changes by pushing your code to your GitHub repository

## Task 6: Set up the Global Store

Navigate to the main.jsx file in the src folder.

The Provider component from the react-redux library is already imported. This component enables all components in the application to access the Redux store.

```javascript
import { Provider } from 'react-redux';
```

The Redux store is imported from the store.js file. This store holds the application's state, using the reducer defined in the CartSlice.jsx file.

```javascript
import store from './store.js';
```

The App component is wrapped with the Provider component, with the Redux store passed as a prop. This allows all components in the app to access and interact with the global state managed by Redux.

```javascript
<Provider store={store}>
  <App />
</Provider>
```




# Deploying Your Application with GitHub Pages
  - 1. To deploy your react application in GitHub you need to install `gh-pages`. This allows you to use it as a tool for deploying your project to GitHub Pages. Perform given command in the terminal

```bash
npm install gh-pages --save-dev
```

  - 2. Add given lines before `"build": "vite build"` in package.json file.
  ```bash
  "predeploy": "npm run build",
   "deploy": "gh-pages -d dist",
   ```
  - 3. Then in the vite.config.js file add this line before plugins: `[react()]`

  ```bash
  base: "/YOUR_REPOSITORY_NAME",
  ```

  - 4. Now perform deploy command in the terminal to executes the "deploy" script defined in the package.json file, deploying the project to GitHub Pages using the gh-pages tool.
  ```bash
    npm run deploy
  ```

  - 5. Perform git add and git commit commands to update changes in your code. Then perform git push command to update your GitHub repository for proper code management.

  - 6. Go to your GitHub repository. Then, navigate to your site's repository that you created.

  - 7. Under your repository name, click Settings.

  - 8.  Navigate to the left hand side navigation bar. In the Code and Automation section of the sidebar, click Pages.

  - 9. You will see the page shown below. Click the drop down menu where you see None, then click gh-pages, and then click the Save button

  - 10. Refresh your page again, and you will see the link, just as below. Instead of shoppingreact, you will see your github repository name.