# Celebal technology



 import React, { useEffect, useState } from "react";
import axios from "axios";
import { Link } from "react-router-dom";

function ProductsPage() {
  const [products, setProducts] = useState([]); // Product list from backend
  const [cart, setCart] = useState(() => {
    // Load cart from localStorage
    return JSON.parse(localStorage.getItem("cart")) || [];
  });

  // Fetch mock data from the backend
  useEffect(() => {
    axios
      .get(`${process.env.REACT_APP_API_URL}/products`)
      .then((response) => setProducts(response.data))
      .catch((error) => console.error("Error fetching products:", error));
  }, []);

  // Add item to cart
  const addToCart = (product) => {
    const updatedCart = [...cart, product];
    setCart(updatedCart);

    // Save the updated cart to localStorage
    localStorage.setItem("cart", JSON.stringify(updatedCart));
  };

  return (
    <div>
      <h1>Products</h1>
      {products.map((product) => (
        <div key={product.id}>
          <p>{product.name} - ${(product.price / 100).toFixed(2)}</p>
          <button onClick={() => addToCart(product)}>Add to Cart</button>
        </div>
      ))}
      <Link to="/cart">
        <button>Go to Cart ({cart.length})</button>
      </Link>
    </div>
  );
}

export default ProductsPage;
