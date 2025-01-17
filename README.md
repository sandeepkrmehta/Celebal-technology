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










import React, { useState, useEffect } from "react";
import { Link } from "react-router-dom";

function CartPage() {
  const [cart, setCart] = useState([]);

  // Load cart data from localStorage
  useEffect(() => {
    const storedCart = JSON.parse(localStorage.getItem("cart")) || [];
    setCart(storedCart);
  }, []);

  // Remove item from cart
  const handleRemove = (index) => {
    const updatedCart = [...cart];
    updatedCart.splice(index, 1); // Remove the item at the specified index
    setCart(updatedCart);

    // Update localStorage after removing the item
    localStorage.setItem("cart", JSON.stringify(updatedCart));
  };

  return (
    <div>
      <h1>Cart</h1>
      {cart.length === 0 ? (
        <p>Your cart is empty</p>
      ) : (
        <div>
          {cart.map((item, index) => (
            <div key={index}>
              <p>{item.name} - ${(item.price / 100).toFixed(2)}</p>
              <button onClick={() => handleRemove(index)}>Remove</button>
            </div>
          ))}
          <Link to="/checkout">
            <button>Proceed to Checkout</button>
          </Link>
        </div>
      )}
    </div>
  );
}

export default CartPage;
