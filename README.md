<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Product Table</title>
  <style>
    :root {
      --one-color: #04aaa3; /* Main color */
      --two-color: #eaeaea; /* Secondary color */
      --three-color: white; /* Accent color */
    }

    * {
      margin: 0;
      padding: 0;
      background-color: var(--two-color);
      box-sizing: border-box;
      font-family: Arial, sans-serif;
    }

    table {
      width: 100%;
      border-collapse: collapse;
      margin: 20px 0;
    }

    th, td {
      padding: 12px;
      text-align: left;
      border: 2px solid black; /* Border color for rows */
    }

    th {
      background-color: var(--one-color);
      color: var(--three-color);
    }

    tr:nth-child(even) {
      background-color: #f9f9f9;
    }

    tr:hover {
      background-color: #f1f1f1;
    }

    #table-container {
      width: 80%;
      margin: 0 auto;
    }

    #loading {
      text-align: center;
      margin: 20px;
      font-size: 18px;
    }
  </style>
</head>
<body>
  <div id="table-container">
    <h1>Product List</h1>
    <div id="loading">Loading products...</div>
    <table id="product-table">
      <thead>
        <tr>
          <th>Product Name</th>
          <th>Price</th>
        </tr>
      </thead>
      <tbody>
      </tbody>
    </table>
  </div>
  
  <script>
    function fetchProducts() {
      var xhr = new XMLHttpRequest();
      xhr.open("GET", "https://openapi.bukaolshop.net/v1/app/produk?token=eyJhcHAiOiIxMjE4MTUiLCJhdXRoIjoiMjAyMjEyMTQiLCJzaWduIjoia1REb3Ztc3RaSktRcFNudk5YXC8zVkE9PSJ9&page=1&total_data=50&id_kategori=379641", true);
      
      xhr.onreadystatechange = function() {
        if (xhr.readyState === XMLHttpRequest.DONE) {
          if (xhr.status === 200) {
            var response = JSON.parse(xhr.responseText);
            var products = response.data;
            var tableBody = document.querySelector("#product-table tbody");
            var loadingDiv = document.getElementById("loading");
            
            // Sort products by price (ascending)
            products.sort(function(a, b) {
              return a.harga_produk - b.harga_produk;
            });

            tableBody.innerHTML = ""; // Clear previous content
            loadingDiv.style.display = "none"; // Hide loading message

            products.forEach(function(product) {
              var row = document.createElement("tr");
              var nameCell = document.createElement("td");
              var priceCell = document.createElement("td");

              nameCell.textContent = product.nama_produk;
              priceCell.textContent = formatCurrency(product.harga_produk);

              row.appendChild(nameCell);
              row.appendChild(priceCell);
              tableBody.appendChild(row);
            });
          } else {
            console.error("Failed to fetch products.");
          }
        }
      };

      xhr.send();
    }

    function formatCurrency(amount) {
      return "Rp " + amount.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ".");
    }

    document.addEventListener("DOMContentLoaded", fetchProducts);
  </script>
</body>
</html>

