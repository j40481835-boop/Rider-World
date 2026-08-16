<!DOCTYPE html>
<html lang="bn">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Rider World</title>

<style>
*{
    margin:0;
    padding:0;
    box-sizing:border-box;
    font-family:Arial,sans-serif;
}

body{
    background:#0b0f14;
    color:white;
}

header{
    height:65px;
    background:#111820;
    display:flex;
    align-items:center;
    justify-content:space-between;
    padding:0 18px;
    position:sticky;
    top:0;
    z-index:1000;
    border-bottom:1px solid #26313c;
}

.logo{
    font-size:22px;
    font-weight:bold;
}

.logo span{
    color:#00e676;
}

.menu{
    font-size:25px;
    cursor:pointer;
}

.container{
    padding:15px;
    max-width:900px;
    margin:auto;
}

.welcome{
    background:linear-gradient(135deg,#00c853,#00695c);
    padding:20px;
    border-radius:18px;
    margin-bottom:15px;
}

.welcome h2{
    margin-bottom:8px;
}

.status{
    display:flex;
    align-items:center;
    gap:8px;
    margin-top:12px;
}

.dot{
    width:11px;
    height:11px;
    background:#00e676;
    border-radius:50%;
}

.card{
    background:#111820;
    border:1px solid #26313c;
    border-radius:18px;
    padding:17px;
    margin-bottom:15px;
}

.card h3{
    margin-bottom:14px;
}

.stats{
    display:grid;
    grid-template-columns:repeat(3,1fr);
    gap:10px;
}

.stat{
    background:#19232d;
    padding:15px 8px;
    text-align:center;
    border-radius:14px;
}

.stat b{
    display:block;
    font-size:20px;
    color:#00e676;
    margin-bottom:5px;
}

#map{
    width:100%;
    height:330px;
    border-radius:15px;
    overflow:hidden;
}

.buttons{
    display:grid;
    grid-template-columns:1fr 1fr;
    gap:10px;
}

button{
    border:0;
    padding:15px;
    border-radius:12px;
    font-size:16px;
    font-weight:bold;
    cursor:pointer;
}

.start{
    background:#00e676;
    color:#001b0d;
}

.stop{
    background:#ff1744;
    color:white;
}

button:active{
    transform:scale(.97);
}

.nav{
    position:fixed;
    bottom:0;
    left:0;
    right:0;
    height:65px;
    background:#111820;
    border-top:1px solid #26313c;
    display:flex;
    justify-content:space-around;
    align-items:center;
}

.nav div{
    text-align:center;
    font-size:12px;
    color:#9aa7b3;
}

.nav .active{
    color:#00e676;
}

.nav-icon{
    font-size:22px;
    display:block;
    margin-bottom:3px;
}

body{
    padding-bottom:70px;
}

@media(max-width:500px){
    .stats{
        grid-template-columns:1fr 1fr 1fr;
    }

    #map{
        height:280px;
    }
}
</style>

<!-- OpenStreetMap / Leaflet -->
<link
rel="stylesheet"
href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css"
/>

</head>

<body>

<header>
    <div class="logo">🌍 Rider <span>World</span></div>
    <div class="menu">☰</div>
</header>

<div class="container">

    <section class="welcome">
        <h2>Welcome, Rider! 🏍️</h2>
        <p>Ready to start your next ride?</p>

        <div class="status">
            <div class="dot"></div>
            <span id="statusText">Online</span>
        </div>
    </section>

    <section class="card">
        <h3>💰 Your Earnings</h3>

        <div class="stats">
            <div class="stat">
                <b id="today">৳0</b>
                Today
            </div>

            <div class="stat">
                <b id="rides">0</b>
                Rides
            </div>

            <div class="stat">
                <b id="total">৳0</b>
                Total
            </div>
        </div>
    </section>

    <section class="card">
        <h3>📍 Rider World Map</h3>
        <div id="map"></div>
    </section>

    <section class="card">
        <h3>🏍️ Ride Control</h3>

        <div class="buttons">
            <button class="start" onclick="startRide()">
                🟢 Start Ride
            </button>

            <button class="stop" onclick="stopRide()">
                🔴 End Ride
            </button>
        </div>
    </section>

    <section class="card">
        <h3>👤 Rider Profile</h3>
        <p>Name: <b>Rider World User</b></p>
        <p style="margin-top:8px;">
            Status: <span id="profileStatus">Online</span>
        </p>
    </section>

</div>

<div class="nav">

    <div class="active">
        <span class="nav-icon">🏠</span>
        Home
    </div>

    <div>
        <span class="nav-icon">🗺️</span>
        Map
    </div>

    <div>
        <span class="nav-icon">💰</span>
        Earnings
    </div>

    <div>
        <span class="nav-icon">👤</span>
        Profile
    </div>

</div>

<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>

<script>

let rideRunning = false;
let rideCount = 0;
let todayMoney = 0;
let totalMoney = 0;

// Map
const map = L.map('map').setView([23.8103,90.4125],6);

L.tileLayer(
    'https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png',
    {
        attribution:'© OpenStreetMap contributors'
    }
).addTo(map);

// Rider marker
let riderMarker = L.marker([23.8103,90.4125])
    .addTo(map)
    .bindPopup("🏍️ Rider World Rider")
    .openPopup();

// Start ride
function startRide(){

    if(rideRunning){
        alert("আপনার Ride ইতিমধ্যে চলছে!");
        return;
    }

    rideRunning = true;

    document.getElementById("statusText").innerText =
        "Riding 🏍️";

    document.getElementById("profileStatus").innerText =
        "Riding 🏍️";

    alert("🏍️ Ride Started!");

    // Demo earning
    setTimeout(function(){

        if(rideRunning){

            let earning = 50;

            todayMoney += earning;
            totalMoney += earning;

            document.getElementById("today").innerText =
                "৳" + todayMoney;

            document.getElementById("total").innerText =
                "৳" + totalMoney;
        }

    },5000);
}

// Stop ride
function stopRide(){

    if(!rideRunning){
        alert("কোনো Ride চলছে না!");
        return;
    }

    rideRunning = false;
    rideCount++;

    document.getElementById("rides").innerText =
        rideCount;

    document.getElementById("statusText").innerText =
        "Online";

    document.getElementById("profileStatus").innerText =
        "Online";

    alert("🔴 Ride Ended!");

}

// Get current location
if(navigator.geolocation){

    navigator.geolocation.watchPosition(

        function(position){

            const lat =
                position.coords.latitude;

            const lng =
                position.coords.longitude;

            riderMarker.setLatLng([lat,lng]);

            map.setView([lat,lng],15);

        },

        function(error){
            console.log("Location unavailable");
        }

    );

}

</script>

</body>
</html>
