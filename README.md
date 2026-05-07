import { useEffect, useState } from "react";
import Product from "./Product";
import "./App.css";

const App = () =>
{

    let [products, setProducts] = useState([]);

    let [title, setTitle] = useState("");

    let [price, setPrice] = useState("");

    let [editId, setEditId] = useState(null);


    useEffect(() =>
    {

        fetch("https://fakestoreapi.com/products")

        .then((res) => res.json())

        .then((res) => setProducts(res))

        .catch((e) => console.log(e))

    }, [])


    const add = () =>
    {

        let data = {

            id: products.length + 1,

            title: title,

            price: price

        }

        setProducts([data, ...products]);

    }


    const remove = (id) =>
    {

        setProducts(

            products.filter((p) => p.id != id)

        )

    }


    const edit = (p) =>
    {

        setTitle(p.title);

        setPrice(p.price);

        setEditId(p.id);

    }


    const update = () =>
    {

        let newProducts = products.map((p) =>
        {

            if(p.id == editId)
            {
                p.title = title;

                p.price = price;
            }

            return p;

        })

        setProducts(newProducts);

        setTitle("");

        setPrice("");

        setEditId(null);

    }


    return(
        <>

            <div className="form">

                <h2>CRUD Operations</h2>

                <input
                    type="text"
                    placeholder="Enter title"
                    value={title}
                    onChange={(e)=> setTitle(e.target.value)}
                />

                <br /><br />

                <input
                    type="number"
                    placeholder="Enter price"
                    value={price}
                    onChange={(e)=> setPrice(e.target.value)}
                />

                <br /><br />

                <button onClick={add}>
                    Add
                </button>

                <button onClick={update}>
                    Update
                </button>

            </div>


            {

                products.map((p)=>

                    <Product

                        id={p.id}

                        title={p.title}

                        price={p.price}

                        remove={remove}

                        edit={edit}

                    />

                )

            }

        </>
    )
}

export default App;


.form
{
    border:2px solid black;
    width:300px;
    padding:20px;
    margin:20px;
    background-color:lightblue;
}

.d1
{
    border:2px solid black;
    width:250px;
    padding:20px;
    margin:20px;
    background-color:lightpink;
    display:inline-block;
}

button
{
    padding:10px;
    margin:5px;
}

input
{
    width:250px;
    padding:10px;
}

import "./App.css";

const Product = ({id, title, price, remove, edit}) =>
{

    return(
        <div className="d1">

            <h2>{id}</h2>

            <h3>{title}</h3>

            <h3>Price : {price}</h3>

            <button onClick={() => remove(id)}>
                Delete
            </button>

            <button onClick={() => edit({id, title, price})}>
                Edit
            </button>

        </div>
    )
}

export default Product;

import React from 'react';
import ReactDOM from 'react-dom/client';
import './index.css';
import App from './App';

const root = ReactDOM.createRoot(document.getElementById('root'));

root.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
);
