# Responsive Google Map

The following uses PHP, HTML, JavaScript, and CSS to create a responsive google map (v3), thereby keeping the map icon and map background layer centered at all times – even if the browser window is resized.

The HTML ```<div id="map-canvas"></div>``` element uses php to output latitude, longitude, and address information that the JavaScript will then use as variables to construct the map and position the map icon marker correctly.

- Be sure to include the ```<script type="text/javascript" src="https://maps.googleapis.com/maps/api/js?v=3.exp&amp;ver=3"></script>```
- Be aware that there are a few additional JavaScript variables being added in this code. These variables grab additional data to construct the ```infowindow```
- Additional documentation can be found at https://developers.google.com/maps/

***


### PHP/HTML

```php
<div id="map-canvas" data-lat="<?php echo $location['lat']; ?>" data-lng="<?php echo $location['lng']; ?>" data-address="<?php echo urlencode($location['address']); ?>"></div>
```

### CSS

```css
#map-canvas {
 	min-width: 300px;
 	width: 100%;
 	height: 300px;
 	margin: 0 auto;
}

#map-canvas img {
  max-width: none;
}
```

### JavaScript
```javascript
var mapID = document.getElementById('map-canvas');

// Get data attributes from #map-canvas for the map
var mapDataLat = mapID.getAttribute('data-lat');
var mapDataLng = mapID.getAttribute('data-lng');
var mapDataAddress = mapID.getAttribute('data-address');

var mbName = document.getElementById('mb-name');
var mbAddress = document.getElementById('mb-address');
var mbCity = document.getElementById('mb-city');
var mbState = document.getElementById('mb-state');
var mbZip = document.getElementById('mb-zip');

var mbDataMBName = mbName.getAttribute('data-mb-name');
var mbDataMBAddress = mbAddress.getAttribute('data-mb-address');
var mbDataMBCity = mbCity.getAttribute('data-mb-city');
var mbDataMBState = mbState.getAttribute('data-mb-state');
var mbDataMBZip= mbZip.getAttribute('data-mb-zip');

function initialize() {
    
    
	var mapOptions = {
		mapTypeId: google.maps.MapTypeId.ROADMAP,
		mapTypeControl: false,
		zoom: 14,
		zoomControl: false,
		panControl: true,
		panControlOptions: {
			position: google.maps.ControlPosition.DEFAULT
		},
		streetViewControl: false,
		scaleControl: false,
		overviewMapControl: false,
		center: new google.maps.LatLng(mapDataLat, mapDataLng)
	};
    	
	map = new google.maps.Map(document.getElementById('map-canvas'),
		mapOptions);
	
	var infoContent = '<div class="window-content"><h4>' + mbDataMBName + '</h4><p>' + mbDataMBAddress + '</p><p>' + mbDataMBCity + ', ' + mbDataMBState + ', ' + mbDataMBZip + '</p><p><a href="https://www.google.com/maps/dir/Current+Location/' + mapDataAddress + '" target="_blank">Directions</a></p></div>';
    
    var infowindow = new google.maps.InfoWindow({
		content: infoContent
	});
	
	var icon = {
		path: 'M16.5,51s-16.5-25.119-16.5-34.327c0-9.2082,7.3873-16.673,16.5-16.673,9.113,0,16.5,7.4648,16.5,16.673,0,9.208-16.5,34.327-16.5,34.327zm0-27.462c3.7523,0,6.7941-3.0737,6.7941-6.8654,0-3.7916-3.0418-6.8654-6.7941-6.8654s-6.7941,3.0737-6.7941,6.8654c0,3.7916,3.0418,6.8654,6.7941,6.8654z',
		anchor: new google.maps.Point(16.5, 51),
		fillColor: '#FF0000',
		fillOpacity: 0.6,
		strokeWeight: 0,
		scale: 0.66
	};
	
	var marker = new google.maps.Marker({
		position: new google.maps.LatLng(mapDataLat, mapDataLng),
		map: map,
		icon: icon,
		title: 'marker'
	});
	
	
	google.maps.event.addListener(marker, 'click', function() {
		infowindow.open(map,marker);
	});
}

google.maps.event.addDomListener(window, 'resize', initialize);
google.maps.event.addDomListener(window, 'load', initialize);
```

