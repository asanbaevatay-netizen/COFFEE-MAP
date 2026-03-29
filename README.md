# COFFEE-MAP
<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <title>Интерактивная карта 2GIS</title>
    <script src="https://maps.api.2gis.ru/2.0/loader.js"></script>
    <style>
        html, body {
            margin: 0;
            padding: 0;
            height: 100%;
        }
        #map {
            width: 100%;
            height: 100%;
        }
        .card-popup {
            width: 250px;
            font-family: Arial, sans-serif;
        }
        .card-popup img {
            width: 100%;
            height: 150px;
            border-radius: 5px;
            object-fit: cover;
        }
        .card-popup h3 {
            margin: 5px 0;
            font-size: 16px;
        }
        .card-popup p {
            margin: 0;
            font-size: 14px;
        }
        .carousel-controls {
            display: flex;
            justify-content: space-between;
            margin-top: 5px;
        }
        .carousel-controls button {
            background-color: #007bff;
            border: none;
            color: white;
            padding: 3px 8px;
            border-radius: 3px;
            cursor: pointer;
        }
    </style>
</head>
<body>

<div id="map"></div>

<script>
DG.then(function () {
    var map = DG.map('map', {
        center: [42.8746, 74.5698],
        zoom: 13
    });

    var locations = [
        {
            coords: [42.8746, 74.5698],
            title: "Центр Бишкека",
            description: "Главная площадь города",
            iconUrl: "https://cdn-icons-png.flaticon.com/512/684/684908.png",
            images: [
                "https://upload.wikimedia.org/wikipedia/commons/4/4d/Bishkek_City_Center.jpg",
                "https://upload.wikimedia.org/wikipedia/commons/8/88/Bishkek_park.jpg"
            ]
        },
        {
            coords: [42.8800, 74.5800],
            title: "CAPITO",
            description: "BEST COFFEE IN THE WORLD",
            iconUrl: "https://cdn-icons-png.flaticon.com/512/138/138292.png",
            images: [
                "https://cdn.pixabay.com/photo/2017/06/15/11/40/coffee-2403631_1280.jpg",
                "https://cdn.pixabay.com/photo/2016/11/18/15/11/coffee-1839626_1280.jpg"
            ]
        },
        {
            coords: [42.8700, 74.5500],
            title: "Кафе",
            description: "Популярное кафе с вкусной едой",
            iconUrl: "https://cdn-icons-png.flaticon.com/512/252/252025.png",
            images: [
                "https://cdn.pixabay.com/photo/2017/06/09/15/09/cafe-2387426_1280.jpg",
                "https://cdn.pixabay.com/photo/2016/03/05/19/02/cafe-1239240_1280.jpg"
            ]
        }
    ];

    locations.forEach(function (place, index) {
        var icon = DG.icon({
            iconUrl: place.iconUrl,
            iconSize: [32, 32],
            iconAnchor: [16, 32]
        });

        var marker = DG.marker(place.coords, {icon: icon}).addTo(map);

        marker.on('click', function () {
            var currentImage = 0;

            function getCardContent() {
                return '<div class="card-popup">'
                    + '<img id="popup-image-' + index + '" src="' + place.images[currentImage] + '" alt="' + place.title + '">' 
                    + '<h3>' + place.title + '</h3>'
                    + '<p>' + place.description + '</p>'
                    + '<div class="carousel-controls">'
                    + '<button id="prev-' + index + '">&lt;</button>'
                    + '<button id="next-' + index + '">&gt;</button>'
                    + '</div>'
                    + '</div>';
            }

            var popup = DG.popup()
                .setLatLng(place.coords)
                .setContent(getCardContent())
                .openOn(map);

            function updateImage() {
                document.getElementById('popup-image-' + index).src = place.images[currentImage];
            }

            document.getElementById('prev-' + index).addEventListener('click', function() {
                currentImage = (currentImage - 1 + place.images.length) % place.images.length;
                updateImage();
            });

            document.getElementById('next-' + index).addEventListener('click', function() {
                currentImage = (currentImage + 1) % place.images.length;
                updateImage();
            });
        });
    });
});
</script>

</body>
</html>
