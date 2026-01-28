# aplikasi-jual-pakaian.html
<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>THAMAM361 | Urban Fashion</title>

<link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600;700&display=swap" rel="stylesheet">
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.0/css/all.min.css">

<style>
:root{
  --primary:#965a2f;
  --dark:#8d2626;
  --bg:#f2f2f2;
}
*{margin:0;padding:0;box-sizing:border-box}
body{font-family:Poppins;background:var(--bg)}
.container{max-width:480px;margin:auto;padding-bottom:120px}

/* HEADER */
header{
  position:sticky;top:0;z-index:10;
  background:linear-gradient(135deg,#000,#222);
  color:#fff;padding:14px;
  border-radius:0 0 20px 20px;
}
.top{
  display:flex;justify-content:space-between;align-items:center
}
.brand{display:flex;align-items:center;gap:10px}
.brand img{width:42px;height:42px;border-radius:50%}
.social{display:flex;gap:14px}
.social a{color:#fff;font-size:18px}

/* PRODUCTS */
.products{
  display:grid;
  grid-template-columns:repeat(2,1fr);
  gap:14px;padding:14px
}
.product{
  background:#fff;
  border-radius:18px;
  padding:10px;
  box-shadow:0 6px 14px rgba(0,0,0,.08)
}
.product img{
  width:100%;height:150px;
  object-fit:cover;border-radius:14px
}
.product h4{font-size:14px;margin:6px 0}
.price{color:var(--primary);font-weight:700}

select,input,textarea,button{
  width:100%;
  padding:9px;
  margin-top:6px;
  border-radius:10px;
  border:1px solid #ddd;
  font-size:13px
}
button{
  border:none;
  background:var(--dark);
  color:#fff
}

/* CART */
.cart{
  background:#fff;
  margin:14px;
  padding:16px;
  border-radius:18px;
  box-shadow:0 6px 16px rgba(0,0,0,.1)
}
.cart-item{
  display:flex;
  justify-content:space-between;
  font-size:13px;
  margin-bottom:6px
}
.cart-item button{
  background:red;
  padding:2px 6px;
  border-radius:6px;
  font-size:11px
}

/* PAYMENT */
.qris{background:#000}
.gopay{background:#00aed6}
.dana{background:#007dff}
.wa{background:#25D366}

/* FLOAT WA */
.sticky-wa{
  position:fixed;
  bottom:16px;left:50%;
  transform:translateX(-50%);
  background:#25D366;
  color:#fff;
  padding:14px 26px;
  border-radius:30px;
  text-decoration:none;
  font-size:14px;
  box-shadow:0 8px 20px rgba(0,0,0,.2)
}
</style>
</head>

<body>
<div class="container">

<header>
  <div class="top">
    <div class="brand">
      <img src="topi.jpg">
      <strong>THAMAM361</strong>
    </div>
    <div class="social">
      <a href="#"><i class="fab fa-tiktok"></i></a>
      <a href="#"><i class="fab fa-instagram"></i></a>
      <a href="#"><i class="fab fa-youtube"></i></a>
    </div>
  </div>
</header>

<!-- PRODUK -->
<div class="products" id="products"></div>

<!-- KERANJANG -->
<div class="cart">
  <h3>🛒 Keranjang</h3>
  <div id="cartList"></div>
  <hr>
  <div class="cart-item">
    <b>Total</b><b id="total">Rp 0</b>
  </div>

  <h4>📦 Alamat Pengiriman</h4>
  <input id="nama" placeholder="Nama">
  <textarea id="alamat" placeholder="Alamat lengkap"></textarea>

  <button class="qris" onclick="alert('QRIS: "085123622237")">QRIS</button>
  <button class="gopay" onclick="alert('GoPay: "085123622237")">GoPay</button>
  <button class="dana" onclick="alert('DANA: "085123622237")">DANA</button>
  <button class="wa" onclick="checkout()">Checkout & Invoice</button>
</div>

</div>

<a class="sticky-wa" href="https://wa.me/085123622237">💬 Chat WhatsApp</a>

<audio id="notif">
  <source src="https://actions.google.com/sounds/v1/cartoon/clang_and_wobble.ogg">
</audio>

<script>
const produk=[
 {nama:"Kaos Oversize",harga:75000,foto:"kaos.jpg"},
 {nama:"Hoodie Street",harga:120000,foto:"hodi.jpg"},
 {nama:"topi",harga:170000,foto:"topi.jpg"}
];

let cart=[];

produk.forEach((p,i)=>{
  products.innerHTML+=`
  <div class="product">
    <img src="${p.foto}">
    <h4>${p.nama}</h4>
    <div class="price">Rp ${p.harga.toLocaleString()}</div>

    <select id="size${i}">
      <option>S</option><option>M</option>
      <option>L</option><option>XL</option>
    </select>

    <input type="number" id="qty${i}" min="1" value="1">

    <button onclick="addCart(${i})">Tambah</button>
  </div>`;
});

function addCart(i){
  let size=document.getElementById("size"+i).value;
  let qty=parseInt(document.getElementById("qty"+i).value);
  cart.push({...produk[i],size,qty});
  document.getElementById("notif").play();
  renderCart();
}

function renderCart(){
  let total=0;
  cartList.innerHTML="";
  cart.forEach((c,i)=>{
    let sub=c.harga*c.qty;
    total+=sub;
    cartList.innerHTML+=`
    <div class="cart-item">
      <span>${c.nama} (${c.size}) x${c.qty}</span>
      <span>Rp ${sub.toLocaleString()}
      <button onclick="removeItem(${i})">x</button></span>
    </div>`;
  });
  document.getElementById("total").innerText="Rp "+total.toLocaleString();
}

function removeItem(i){
  cart.splice(i,1);
  renderCart();
}

function checkout(){
  let nama=document.getElementById("nama").value;
  let alamat=document.getElementById("alamat").value;
  let invoice="INV"+Date.now();

  let text=`INVOICE: ${invoice}%0A`+
           `Nama: ${nama}%0AAlamat: ${alamat}%0A%0AOrder:%0A`;

  let total=0;
  cart.forEach(c=>{
    let sub=c.harga*c.qty;
    total+=sub;
    text+=`- ${c.nama} (${c.size}) x${c.qty}%0A`;
  });

  text+=`%0ATotal: Rp ${total.toLocaleString()}`;
  window.open(`https://wa.me/085123622237?text=${text}`);
}
</script>

</body>
</html>
