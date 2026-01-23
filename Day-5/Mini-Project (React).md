
### `ProductCard.jsx`

```jsx
const ProductCard = ({ product }) => {
  return (
    <div className="border rounded p-4">
      <img
        src={product.image}
        alt={product.title}
        className="h-40 w-full object-contain mb-2"
      />
      <h2 className="text-sm font-medium">{product.title}</h2>
      <p className="text-gray-600">${product.price}</p>
    </div>
  );
};

export default ProductCard;
```

---

### `App.jsx`

```jsx
import { useEffect, useState } from "react";
import ProductCard from "./ProductCard";

function App() {
  const [products, setProducts] = useState([]);

  useEffect(() => {
    const fetchProducts = async () => {
      const response = await fetch("https://fakestoreapi.com/products");
      const data = await response.json();
      setProducts(data);
    };

    fetchProducts();
  }, []);

  return (
    <div className="p-4 grid grid-cols-1 sm:grid-cols-2 md:grid-cols-4 gap-4">
      {products.map((product) => (
        <ProductCard key={product.id} product={product} />
      ))}
    </div>
  );
}

export default App;
```

---

## What is Routing in React?

Routing allows you to show **different pages (components)** based on the **URL**, without reloading the page.

Example:

- `/` → Home page
    
- `/products` → Products page
    
- `/product/5` → Single product page
    

React Router DOM handles this for **single-page applications (SPA)**.

---

## Install React Router DOM

```bash
npm install react-router-dom
```

---

## Basic Setup (BrowserRouter)

Wrap your app with `BrowserRouter`.

### `main.jsx` or `index.js`

```jsx
import React from "react";
import ReactDOM from "react-dom/client";
import { BrowserRouter } from "react-router-dom";
import App from "./App";

ReactDOM.createRoot(document.getElementById("root")).render(
  <BrowserRouter>
    <Routes>
      <Route path="/" element={<App />} />
      <Route path="/products" element={<Products />} />
      <Route path="/product/:id" element={<ProductDetails />} />
    </Routes>
  </BrowserRouter>
);
```

---

### Key points:

- `path` → URL
    
- `element` → component to render
    
- `:id` → **dynamic route parameter**
    

---

## Navigation using Link

Use `Link` instead of `<a>` (prevents page reload).

```jsx
import { Link } from "react-router-dom";

const Navbar = () => {
  return (
    <nav>
      <Link to="/">Home</Link>
      <Link to="/products">Products</Link>
    </nav>
  );
};

export default Navbar;
```

---

## Dynamic Routing (useParams)

Access route parameters like `id`.

### `ProductDetails.jsx`

```jsx
import { useParams } from "react-router-dom";

const ProductDetails = () => {
  const { id } = useParams();

  return <h1>Product ID: {id}</h1>;
};

export default ProductDetails;
```

Example URL:

```
/product/3
```

➡️ `id = 3`

---

## Programmatic Navigation (useNavigate)

Navigate using JavaScript (on button click, after submit, etc.).

```jsx
import { useNavigate } from "react-router-dom";

const Home = () => {
  const navigate = useNavigate();

  return (
    <button onClick={() => navigate("/products")}>
      Go to Products
    </button>
  );
};

export default Home;
```

---

## 404 Page (Optional)

```jsx
<Route path="*" element={<h1>Page Not Found</h1>} />
```

---