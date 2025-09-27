# gudangks.github.io
<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Sistem Kasir & Stok Sederhana</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
            background-color: #f4f4f4;
            
            color: #333;
        }
        .container {
            max-width: 900px;
            margin: 20px auto;
            padding: 20px;
            background-color: #fff;
            border-radius: 8px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
        }
        h1, h2 {
            color: #007bff;
        }
        .login-page, .admin-page, .kasir-page {
            display: none;
        }
        .active {
            display: block;
        }
        .form-group {
            margin-bottom: 15px;
        }
        .form-group label {
            display: block;
            margin-bottom: 5px;
        }
        .form-group input, .form-group select {
            width: 100%;
            padding: 8px;
            box-sizing: border-box;
            border: 1px solid #ccc;
            border-radius: 4px;
        }
        .btn {
            padding: 10px 15px;
            border: none;
            border-radius: 4px;
            cursor: pointer;
            color: #fff;
            font-weight: bold;
            transition: background-color 0.3s;
        }
        .btn-primary {
            background-color: #007bff;
        }
        .btn-primary:hover {
            background-color: #0056b3;
        }
        .btn-danger {
            background-color: #dc3545;
        }
        .btn-danger:hover {
            background-color: #c82333;
        }
        .btn-warning {
            background-color: #ffc107;
        }
        .btn-warning:hover {
            background-color: #e0a800;
        }
        table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 20px;
        }
        th, td {
            border: 1px solid #ddd;
            padding: 8px;
            text-align: left;
        }
        th {
            background-color: #f2f2f2;
        }
        .kasir-container {
            display: flex;
            gap: 20px;
        }
        .product-list, .cart-section {
            flex: 1;
        }
        .product-item {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 10px;
            border: 1px solid #eee;
            margin-bottom: 5px;
            cursor: pointer;
        }
        .product-item:hover {
            background-color: #f9f9f9;
        }
        .cart-item {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 5px;
        }
        .cart-item button {
            margin-left: 10px;
        }
    </style>
</head>
<body>

<div class="container">
    
    <!-- Login Page -->
    <div id="loginPage" class="login-page active">
        <h1>WELCOME TO KOPERASI MERAH PUTIH
        KAMPUNG PARADOI</h1>
        
        <h2>SILAHKAN LOGIN</h2>
        <div class="form-group">
            <label for="username">Nama Pengguna:</label>
            <input type="text" id="username">
        </div>
        <div class="form-group">
            <label for="password">Kata Sandi:</label>
            <input type="password" id="password">
        </div>
        <button class="btn btn-primary" onclick="login()">Login</button>
        <p style="margin-top: 10px; font-size: 12px; color: #666;">
            Admin: admin / kikysta<br>
            Kasir: kasir / satya
        </p>
    </div>


    <!-- Admin Page -->
    <div id="adminPage" class="admin-page">
        <button class="btn btn-danger" style="float: right;" onclick="logout()">Logout</button>
        <h1>Manajemen Stok Admin</h1>

        <h2>Tambah/Edit Produk</h2>
        <form id="productForm">
            <input type="hidden" id="productId">
            <div class="form-group">
                <label for="productName">Nama Produk:</label>
                <input type="text" id="productName" required>
            </div>
            <div class="form-group">
                <label for="productBarcode">Kode Barcode:</label>
                <input type="text" id="productBarcode" required>
            </div>
            <div class="form-group">
                <label for="purchasePrice">Harga Beli:</label>
                <input type="number" id="purchasePrice" min="0" required>
            </div>
            <div class="form-group">
                <label for="sellingPrice">Harga Jual:</label>
                <input type="number" id="sellingPrice" min="0" required>
            </div>
            <div class="form-group">
                <label for="stock">Stok:</label>
                <input type="number" id="stock" min="0" required>
            </div>
            <button class="btn btn-primary" type="submit">Simpan Produk</button>
            <button class="btn btn-warning" type="button" onclick="clearForm()">Bersihkan</button>
        </form>

        <h2>Daftar Produk</h2>
        <table>
            <thead>
                <tr>
                    <th>Nama</th>
                    <th>Barcode</th>
                    <th>Harga Beli</th>
                    <th>Harga Jual</th>
                    <th>Stok</th>
                    <th>Aksi</th>
                </tr>
            </thead>
            <tbody id="productsTableBody"></tbody>
        </table>
    </div>

    <!-- Kasir Page -->
    <div id="kasirPage" class="kasir-page">
        <button class="btn btn-danger" style="float: right;" onclick="logout()">Logout</button>
        <h1>Aplikasi Kasir</h1>
        
        <div class="kasir-container">
            <div class="product-list">
                <h2>Cari Produk</h2>
                <div class="form-group">
                    <label for="barcodeInput">Pemindai Barcode:</label>
                    <input type="text" id="barcodeInput" placeholder="Pindai barcode...">
                </div>
                <div id="productSearchResults"></div>
            </div>

            <div class="cart-section">
                <h2>Keranjang Belanja</h2>
                <div id="cartItems"></div>
                <hr style="margin: 15px 0;">
                <p>Total: <span id="cartTotal">Rp 0</span></p>
                
                <div class="form-group">
                    <label for="paidAmount">Jumlah Dibayar:</label>
                    <input type="number" id="paidAmount" min="0">
                </div>
                <p>Kembalian: <span id="changeAmount">Rp 0</span></p>

                <button class="btn btn-primary" onclick="checkout()">Bayar</button>
                <button class="btn btn-warning" onclick="resetCart()">Bersihkan Keranjang</button>
            </div>
        </div>
    </div>
</div>

<script>
    const users = {
        'admin': 'admin',
        'kasir': 'kasir'
    };
    let products = JSON.parse(localStorage.getItem('products')) || [];
    let cart = [];
    let transactionHistory = JSON.parse(localStorage.getItem('transactions')) || [];

    const loginPage = document.getElementById('loginPage');
    const adminPage = document.getElementById('adminPage');
    const kasirPage = document.getElementById('kasirPage');
    const productsTableBody = document.getElementById('productsTableBody');
    const productForm = document.getElementById('productForm');
    const productSearchResults = document.getElementById('productSearchResults');
    const cartItemsDiv = document.getElementById('cartItems');
    const cartTotalSpan = document.getElementById('cartTotal');
    const paidAmountInput = document.getElementById('paidAmount');
    const changeAmountSpan = document.getElementById('changeAmount');

    function login() {
        const username = document.getElementById('username').value;
        const password = document.getElementById('password').value;

        if (users[username] && users[username] === password) {
            loginPage.classList.remove('active');
            if (username === 'admin') {
                adminPage.classList.add('active');
                renderAdminProducts();
            } else if (username === 'kasir') {
                kasirPage.classList.add('active');
                setupKasirListeners();
            }
        } else {
            alert('Nama pengguna atau kata sandi salah.');
        }
    }

    function logout() {
        if (confirm('Apakah Anda yakin ingin logout?')) {
            loginPage.classList.add('active');
            adminPage.classList.remove('active');
            kasirPage.classList.remove('active');
            document.getElementById('username').value = '';
            document.getElementById('password').value = '';
            resetKasir();
        }
    }

    // --- Admin Functions ---
    function renderAdminProducts() {
        productsTableBody.innerHTML = '';
        products.forEach(product => {
            const row = document.createElement('tr');
            row.innerHTML = `
                <td>${product.name}</td>
                <td>${product.barcode}</td>
                <td>Rp ${product.purchasePrice.toLocaleString('id-ID')}</td>
                <td>Rp ${product.sellingPrice.toLocaleString('id-ID')}</td>
                <td>${product.stock}</td>
                <td>
                    <button class="btn btn-warning" onclick="editProduct('${product.id}')">Edit</button>
                    <button class="btn btn-danger" onclick="deleteProduct('${product.id}')">Hapus</button>
                </td>
            `;
            productsTableBody.appendChild(row);
        });
    }

    productForm.addEventListener('submit', function(event) {
        event.preventDefault();
        const productId = document.getElementById('productId').value;
        const name = document.getElementById('productName').value;
        const barcode = document.getElementById('productBarcode').value;
        const purchasePrice = parseFloat(document.getElementById('purchasePrice').value);
        const sellingPrice = parseFloat(document.getElementById('sellingPrice').value);
        const stock = parseInt(document.getElementById('stock').value);

        if (productId) {
            // Edit existing product
            const index = products.findIndex(p => p.id === productId);
            if (index !== -1) {
                products[index] = { id: productId, name, barcode, purchasePrice, sellingPrice, stock };
            }
        } else {
            // Add new product
            const newProduct = {
                id: Date.now().toString(),
                name,
                barcode,
                purchasePrice,
                sellingPrice,
                stock
            };
            products.push(newProduct);
        }

        saveProducts();
        renderAdminProducts();
        clearForm();
    });

    function editProduct(id) {
        const product = products.find(p => p.id === id);
        if (product) {
            document.getElementById('productId').value = product.id;
            document.getElementById('productName').value = product.name;
            document.getElementById('productBarcode').value = product.barcode;
            document.getElementById('purchasePrice').value = product.purchasePrice;
            document.getElementById('sellingPrice').value = product.sellingPrice;
            document.getElementById('stock').value = product.stock;
        }
    }

    function deleteProduct(id) {
        if (confirm('Apakah Anda yakin ingin menghapus produk ini?')) {
            products = products.filter(p => p.id !== id);
            saveProducts();
            renderAdminProducts();
        }
    }

    function clearForm() {
        productForm.reset();
        document.getElementById('productId').value = '';
    }

    function saveProducts() {
        localStorage.setItem('products', JSON.stringify(products));
    }

    // --- Kasir Functions ---
    function setupKasirListeners() {
        document.getElementById('barcodeInput').addEventListener('input', function() {
            const query = this.value.toLowerCase();
            productSearchResults.innerHTML = '';
            const foundProducts = products.filter(p => 
                p.barcode.toLowerCase().includes(query) || p.name.toLowerCase().includes(query)
            );
            
            if (foundProducts.length > 0) {
                foundProducts.forEach(p => {
                    const item = document.createElement('div');
                    item.className = 'product-item';
                    item.innerHTML = `
                        <span>${p.name}</span>
                        <span>Rp ${p.sellingPrice.toLocaleString('id-ID')}</span>
                    `;
                    item.onclick = () => addToCart(p.id);
                    productSearchResults.appendChild(item);
                });
            } else {
                productSearchResults.innerHTML = '<p>Produk tidak ditemukan.</p>';
            }
        });

        paidAmountInput.addEventListener('input', updateChange);
    }

    function addToCart(productId) {
        const product = products.find(p => p.id === productId);
        if (!product) return;
        
        const cartItem = cart.find(item => item.id === productId);
        if (cartItem) {
            cartItem.quantity++;
        } else {
            cart.push({ ...product, quantity: 1 });
        }
        updateCart();
    }

    function updateCart() {
        cartItemsDiv.innerHTML = '';
        let total = 0;
        cart.forEach(item => {
            const itemDiv = document.createElement('div');
            itemDiv.className = 'cart-item';
            itemDiv.innerHTML = `
                <span>${item.name} (${item.quantity})</span>
                <span>Rp ${(item.sellingPrice * item.quantity).toLocaleString('id-ID')}</span>
                <button class="btn btn-danger" onclick="removeFromCart('${item.id}')">Hapus</button>
            `;
            cartItemsDiv.appendChild(itemDiv);
            total += item.sellingPrice * item.quantity;
        });
        cartTotalSpan.textContent = `Rp ${total.toLocaleString('id-ID')}`;
        updateChange();
    }

    function removeFromCart(productId) {
        const index = cart.findIndex(item => item.id === productId);
        if (index !== -1) {
            cart[index].quantity--;
            if (cart[index].quantity === 0) {
                cart.splice(index, 1);
            }
        }
        updateCart();
    }

    function updateChange() {
        const total = cart.reduce((sum, item) => sum + item.sellingPrice * item.quantity, 0);
        const paid = parseFloat(paidAmountInput.value) || 0;
        const change = paid - total;
        changeAmountSpan.textContent = `Rp ${Math.max(0, change).toLocaleString('id-ID')}`;
    }

    function checkout() {
        const total = cart.reduce((sum, item) => sum + item.sellingPrice * item.quantity, 0);
        const paid = parseFloat(paidAmountInput.value) || 0;

        if (cart.length === 0) {
            alert('Keranjang belanja kosong.');
            return;
        }

        if (paid < total) {
            alert('Jumlah yang dibayarkan tidak mencukupi.');
            return;
        }

        // Update stock
        cart.forEach(cartItem => {
            const product = products.find(p => p.id === cartItem.id);
            if (product) {
                product.stock -= cartItem.quantity;
            }
        });
        saveProducts();

        const transaction = {
            id: Date.now(),
            items: cart,
            total: total,
            paid: paid,
            change: paid - total,
            timestamp: new Date().toLocaleString()
        };
        transactionHistory.push(transaction);
        localStorage.setItem('transactions', JSON.stringify(transactionHistory));

        generateReceipt(transaction);
        resetKasir();
    }
    
    function resetKasir() {
        cart = [];
        updateCart();
        paidAmountInput.value = '';
        document.getElementById('barcodeInput').value = '';
        productSearchResults.innerHTML = '';
    }

    function generateReceipt(transaction) {
        // Create the receipt content
        const receiptContent = `
            <div style="width: 300px; margin: 0 auto; font-family: 'Courier New', monospace; font-size: 14px; line-height: 1.5;">
                <h3 style="text-align: center;">STRUK PEMBELIAN</h3>
                <hr style="border-top: 1px dashed black;">
                <p>Tanggal: ${transaction.timestamp}</p>
                <hr style="border-top: 1px dashed black;">
                ${transaction.items.map(item => `
                    <div style="display: flex; justify-content: space-between;">
                        <span>${item.name} x ${item.quantity}</span>
                        <span>Rp ${(item.sellingPrice * item.quantity).toLocaleString('id-ID')}</span>
                    </div>
                `).join('')}
                <hr style="border-top: 1px dashed black;">
                <div style="display: flex; justify-content: space-between;">
                    <span>Total:</span>
                    <span>Rp ${transaction.total.toLocaleString('id-ID')}</span>
                </div>
                <div style="display: flex; justify-content: space-between;">
                    <span>Dibayar:</span>
                    <span>Rp ${transaction.paid.toLocaleString('id-ID')}</span>
                </div>
                <div style="display: flex; justify-content: space-between;">
                    <span>Kembalian:</span>
                    <span>Rp ${transaction.change.toLocaleString('id-ID')}</span>
                </div>
                <hr style="border-top: 1px dashed black;">
                <p style="text-align: center;">Terima kasih atas kunjungan Anda!</p>
            </div>
        `;

        // Create a temporary window to print
        const printWindow = window.open('', '_blank');
        printWindow.document.write('<html><head><title>Struk Pembelian</title></head><body>');
        printWindow.document.write(receiptContent);
        printWindow.document.write('</body></html>');
        printWindow.document.close();
        printWindow.onload = () => printWindow.print();
    }
</script>

</body>
</html>

