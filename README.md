<!DOCTYPE html><html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Jewellery Shop</title>
  <style>
    body { font-family: Arial, sans-serif; margin: 0; padding: 0; background: #f4e9ce; }
    header { background: linear-gradient(90deg,#b88932,#e6c372,#b88932); padding: 25px; color: white; text-align: center; font-size: 24px; border-bottom: 4px solid #8a6b24; }
    .banner { width:100%; height:220px; background:url('banner.jpg'); background-size:cover; background-position:center; border-bottom:4px solid #8a6b24; }
    .container { width: 90%; margin: auto; padding: 20px; }
    .price-box { display: flex; gap: 20px; margin-bottom: 20px; }
    .box { background: #fff7dd; padding: 15px; border-radius: 8px; width: 220px; box-shadow: 0 2px 6px rgba(0,0,0,0.3); border:2px solid #c9a23a; }
    .product-upload { background: #fff7dd; padding: 20px; border-radius: 10px; box-shadow: 0 2px 6px rgba(0,0,0,0.3); border:2px solid #c9a23a; margin-bottom: 20px; }
    .product-upload input, .product-upload textarea { width: 100%; padding: 10px; margin: 8px 0; border-radius: 6px; border: 1px solid #b89134; background:#fff; }
    .product-upload button { padding: 10px 20px; background: #b88932; color: white; border: none; border-radius: 6px; cursor: pointer; }
    .products { display: grid; grid-template-columns: repeat(auto-fill, minmax(200px, 1fr)); gap: 20px; }
    .item { background: #fff7dd; padding: 10px; border-radius: 8px; box-shadow: 0 2px 6px rgba(0,0,0,0.3); border:2px solid #c9a23a; }
    footer { background:#b88932; color:white; padding:20px; text-align:center; margin-top:30px; border-top:4px solid #8a6b24; }
    #loginPanel { display:none; background:#fff3cc; padding:20px; border:2px solid #c9a23a; width:300px; margin:auto; border-radius:10px; box-shadow:0 2px 6px rgba(0,0,0,0.3); }
  .zoomImg { width:100%; border-radius:6px; cursor:pointer; }
  .zoomImg:hover { transform: scale(1.7); z-index:10; position:relative; }
</style>
</head>
<body>
<header>
  <div style="position:absolute; top:10px; right:10px;">
    <select id="langSelect" onchange="changeLang()" style="padding:6px; border-radius:6px; border:2px solid #b88932;">
      <option value="en">English</option>
      <option value="hi">हिन्दी</option>
    </select>
  </div>
  <div style="font-size:32px; font-weight:bold; text-shadow:1px 1px 3px black;">Shaweta Jewellers Shop</div>
  <div style="font-size:16px;">Paiga Road, Jalalpur, Bheldi (Saran)</div>
</header>
<div style="text-align:center; padding:15px;">
  <img src="logo.png" alt="Shop Logo" style="height:110px; border-radius:10px; border:3px solid #b88932; box-shadow:0 2px 6px rgba(0,0,0,0.3);">
</div>
<div class="banner""></div><div class="container">  <!-- Gold & Silver Price Update --><h2>Live Rate Panel</h2>
<div class="price-box" style="margin-bottom:30px;">
  <div class="box">
    <h3>All India Gold Price<br><small>(Auto Update)</small></h3>
    <div id="goldLive">Loading...</div>
  </div>
  <div class="box">
    <h3>All India Silver Price<br><small>(Auto Update)</small></h3>
    <div id="silverLive">Loading...</div>
  </div>
</div><h2>Shop Rate Panel</h2>
<div class="price-box">
  <div class="box">
    <h3>Shop Gold Rate</h3>
    <input type="number" id="goldPrice" placeholder="₹ per gram" />
  </div>
  <div class="box">
    <h3>Shop Silver Rate</h3>
    <input type="number" id="silverPrice" placeholder="₹ per gram" />
  </div>
</div>
  <div class="price-box">
    <div class="box">
      <h3>Gold Price</h3>
      <input type="number" id="goldPrice" placeholder="₹ per gram" />
    </div>
    <div class="box">
      <h3>Silver Price</h3>
      <input type="number" id="silverPrice" placeholder="₹ per gram" />
    </div>
  </div>  <!-- Product Upload -->  <div class="product-upload">
    <h2>Add New Jewellery Item</h2>
    <input type="text" id="pname" placeholder="Product Name" />
    <input type="text" id="pweight" placeholder="Weight (in grams)" />
    <input type="number" id="pprice" placeholder="Price" />
    <textarea id="pdesc" placeholder="Description"></textarea>
    <input type="file" id="pimage" accept="image/*" />
    <button onclick="addProduct()">Add Product</button>
  </div>  <!-- Product List -->  <h2 style='margin-top:40px;'>Categories</h2>
<div style='display:flex; gap:15px; overflow-x:auto; padding-bottom:10px;'>
  <div class='box' style='min-width:150px; text-align:center;'>Gold Rings</div>
  <div class='box' style='min-width:150px; text-align:center;'>Mangalsutra</div>
  <div class='box' style='min-width:150px; text-align:center;'>Chains</div>
  <div class='box' style='min-width:150px; text-align:center;'>Silver Payal</div>
  <div class='box' style='min-width:150px; text-align:center;'>Earrings</div>
</div><h2>Products</h2>
<div class="products" id="productList">
    <div class="item">
        <img src="..." class="zoomImg" />
        <h4>Product Name</h4>
        <p>₹0000</p>
        <small>Product description...</small>
    </div>
</div></div></div><script>
  function addProduct() {
    const name = document.getElementById("pname").value;
    const price = document.getElementById("pprice").value;
    const desc = document.getElementById("pdesc").value;
    const image = document.getElementById("pimage").files[0];

    if (!name || !price || !desc || !image) {
      alert("Please fill all fields!");
      return;
    }

    const reader = new FileReader();
    reader.onload = function (e) {
      const div = document.createElement("div");
      div.className = "item";
      div.innerHTML = `
        <img src="${e.target.result}" class="zoomImg" />
        <h4>${name}</h4>
        <p><b>Weight:</b> ${document.getElementById('pweight').value} gm</p>
        <p><b>Price:</b> ₹${price}</p>
        <small>${desc}</small>
      `;
      document.getElementById("productList").appendChild(div);
    };
    reader.readAsDataURL(image);

    document.getElementById("pname").value = "";
    document.getElementById("pprice").value = "";
    document.getElementById("pdesc").value = "";
    document.getElementById("pimage").value = "";
  }
// Auto Live Gold/Silver Rate Fetch (Demo API)
setInterval(() => {
  document.getElementById("goldLive").innerText = "₹ " + (6000 + Math.floor(Math.random()*50)) + " / gram";
  document.getElementById("silverLive").innerText = "₹ " + (75 + Math.floor(Math.random()*5)) + " / gram";
}, 3000);

// 🔐 SECURE LOGIN SYSTEM
let ADMIN_USER = "admin";
let ADMIN_PASS = "Skk@9262";
let isLogged = false;

function openLogin() {
  document.getElementById('loginPanel').style.display='block';
}

function adminLogin(){
  let user = document.getElementById('adminUser').value;
  let pass = document.getElementById('adminPass').value;

  if(user === ADMIN_USER && pass === ADMIN_PASS){
      isLogged = true;
      alert('Admin Login Successful');
      document.getElementById('adminArea').style.display='block';
      document.getElementById('loginPanel').style.display='none';
  } else {
      alert('Wrong Username or Password');
  }
}

// 🕵 Secret Key to Open Login (Press "L")
document.addEventListener('keydown', function(e){
  if(e.key === 'l' || e.key === 'L'){
    openLogin();
  }
});

// Auto Live Gold/Silver Rate Fetch (Demo API)/Silver Rate Fetch (Demo API)
setInterval(() => {
  document.getElementById("goldLive").innerText = "₹ " + (6000 + Math.floor(Math.random()*50)) + " / gram";
  document.getElementById("silverLive").innerText = "₹ " + (75 + Math.floor(Math.random()*5)) + " / gram";
}, 3000);
function changeLang(){
  let lang = document.getElementById('langSelect').value;
  let texts = document.querySelectorAll('[data-en]');
  texts.forEach(t=>{
    t.innerText = t.getAttribute(`data-${lang}`);
  });
}
</script><div id="loginPanel">
  <h3>Admin Login</h3>
  <input type="text" id="adminUser" placeholder="Enter Username" />
  <input type="password" id="adminPass" placeholder="Enter Password" />
  <button onclick="adminLogin()">Login</button>  <div id='adminArea' style='display:none; margin-top:15px;'>
    <h3>Admin Control Panel</h3>
    <button onclick="alert('Product Editing Panel Opened')">Manage Products (Edit/Delete)</button><br><br>
    <button onclick="alert('Rate Update Panel Opened')">Update Rate Panel</button><br><br>
    <button onclick="alert('Banner Change Panel Opened')">Change Banner</button>
  </div>
</div>
</div><footer>
  Shaweta Jewellers Shop • Paiga Road, Jalalpur, Bheldi (Saran)
</footer>
<!-- Image Popup Modal -->
<div id="imgModal" style="display:none; position:fixed; top:0; left:0; width:100%; height:100%; background:rgba(0,0,0,0.8); justify-content:center; align-items:center; z-index:1000;">
  <img id="modalImg" src="" style="max-width:90%; max-height:90%; border:4px solid #fff; border-radius:10px;" />
</div><script>
// Click to open gallery modal
setTimeout(() => {
  document.querySelectorAll('.zoomImg').forEach(img => {
    img.addEventListener('click', function(){
      document.getElementById('modalImg').src = this.src;
      document.getElementById('imgModal').style.display = 'flex';
    });
  });
}, 500);

document.getElementById('imgModal').addEventListener('click', function(){
  this.style.display = 'none';
});
</script></body>
</html>
