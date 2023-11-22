let products = [];
let selectedIndex = -1; // Track the selected index for updating

// Check if product data is available in local storage
if (localStorage.getItem('products')) {
    products = JSON.parse(localStorage.getItem('products'));
    displayProductList();
}

function addProduct() {
    const productNameInput = document.getElementById("productName");
    const sellingPriceInput = document.getElementById("sellingPrice");

    const productName = productNameInput.value;
    const sellingPrice = parseFloat(sellingPriceInput.value);

    if (productName && !isNaN(sellingPrice)) {
        // Push the product to the local array for display
        products.push({ productName, sellingPrice });

        // Save the product data in local storage
        localStorage.setItem('products', JSON.stringify(products));

        // Clear input fields
        productNameInput.value = "";
        sellingPriceInput.value = "";

        // Display the product list with the new item
        displayProductList();

        // Store the product data in the database using Axios
        storeProductData({ productName, sellingPrice });
    }
}

function updateProduct() {
    const updateProductNameInput = document.getElementById("updateProductName");
    const updateSellingPriceInput = document.getElementById("updateSellingPrice");

    const updatedProductName = updateProductNameInput.value;
    const updatedSellingPrice = parseFloat(updateSellingPriceInput.value);

    if (updatedProductName && !isNaN(updatedSellingPrice) && selectedIndex !== -1) {
        // Update the product data at the selected index
        products[selectedIndex] = { productName: updatedProductName, sellingPrice: updatedSellingPrice };

        // Save the updated product data in local storage
        localStorage.setItem('products', JSON.stringify(products));

        // Clear input fields
        updateProductNameInput.value = "";
        updateSellingPriceInput.value = "";

        // Display the updated product list
        displayProductList();

        // Clear the selected index for updating
        selectedIndex = -1;
    }
}

function deleteProduct(index) {
    if (index >= 0 && index < products.length) {
        // Remove the product from the list
        products.splice(index, 1);

        // Save the updated product list to local storage
        localStorage.setItem('products', JSON.stringify(products));

        // Display the updated product list
        displayProductList();

        // Delete the product data from the database using Axios
        deleteProductData(index);
    }
}

function displayProductList() {
    const productList = document.getElementById("product-list");
    productList.innerHTML = '';

    products.forEach((product, index) => {
        const listItem = document.createElement("li");
        listItem.textContent = `${product.productName} - $${product.sellingPrice.toFixed(2)}`;

        // Add an "Edit" button for each product
        const editButton = document.createElement("button");
        editButton.textContent = "Edit";
        editButton.onclick = () => editProduct(index);

        // Add a "Delete" button for each product
        const deleteButton = document.createElement("button");
        deleteButton.textContent = "Delete";
        deleteButton.onclick = () => deleteProduct(index);

        listItem.appendChild(editButton);
        listItem.appendChild(deleteButton);
        productList.appendChild(listItem);
    });
}

function displayUserInput() {
    const userNameInput = document.getElementById("userName");
    const displayedUserInput = document.getElementById("displayedUserInput");

    const userName = userNameInput.value;

    if (userName) {
        // Display the user input
        displayedUserInput.textContent = `User Name: ${userName}`;

        // You can perform additional actions or send the user input to the server here

        // Clear the input field
        userNameInput.value = "";
    }
}

// Function to store product data in the database using Axios
function storeProductData(productData) {
    // For the sake of example, assuming a placeholder URL
    axios.post("https://example.com/api/products", productData)
        .then((response) => {
            console.log('Data stored successfully:', response.data);
        })
        .catch((error) => {
            console.error('Error storing data:', error);
        });
}

// Function to delete product data from the database using Axios
function deleteProductData(index) {
    // For the sake of example, assuming a placeholder URL
    axios.delete(`https://example.com/api/products/${index}`)
        .then((response) => {
            console.log('Data deleted successfully:', response.data);
        })
        .catch((error) => {
            console.error('Error deleting data:', error);
        });
}
