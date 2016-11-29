# Google Maps Abe Recipe
This Abe recipe demonstrates how to add a Google Maps field to your template and display a Google Map


# Installation
1. git clone https://github.com/Abejs/recipe-googlemaps.git
2. cd recipe-googlemaps
3. abe serve
4. open your browser at the address : http://localhost:3000/abe/editor
5. Enjoy !

# Description
In this demo, you see 
- a Google Maps autocomplete field in the editor
- a Google Map filled with geolocations in the template

# The autocomplete field
``` 
{{abe type='data' key='gmaps' source="https://maps.googleapis.com/maps/api/geocode/json?key=YOURKEY&address=" autocomplete="true" display="{{formatted_address}} - (lat:{{geometry.location.lat}}-lng:{{geometry.location.lng}})" desc='gmaps'}}
```
The Abe type used is a data type. In the source attribute, put the url of Gmaps geocode search with the "address" querystring empty. It will be filled in with your autocomplete data. 
Add the attribute autocomplete="true" and chose the data extracted from the gmaps json you want to display to the user. In the example above, we illustrate the possibility to add many different fields of the json: {{formatted_address}}, {{geometry.location.lat}} and {{geometry.location.lng}}

That's all : With this simple Abe tag, your users will enjoy the Google geolocation feature directly from Abe editor!
The data selected by the user will be saved with the document.

# Display a Google Map in the template
The code sample :
```
<div id="map"></div>
{{#if gmaps.0.formatted_address}}
    <script>
      function initMap() {
	var map = new google.maps.Map(document.getElementById('map'), {
	  zoom: 4,
	  center: new google.maps.LatLng(
		{{gmaps.0.geometry.location.lat}},
		{{gmaps.0.geometry.location.lng}}
	  )
	});
	{{#each gmaps}}
	new google.maps.Marker({
	  position: new google.maps.LatLng(
		{{geometry.location.lat}},
		{{geometry.location.lng}}
	  ),
	  map: map
	});
	{{/each}}
      }
    </script>
    <script async defer src="https://maps.googleapis.com/maps/api/js?key=YOURKEY&callback=initMap"></script>
{{/if}}
```
The Gmaps code has been taken from the Google Maps site dev site. In this sample, we don't display the map if there is no record to display ({{#if gmaps.0.formatted_address}})
Then we center the map on the first record : 
``` 
  center: new google.maps.LatLng(
    {{gmaps.0.geometry.location.lat}},
    {{gmaps.0.geometry.location.lng}}
  ) 
```
Finally, we parse each record and create a Marker:
```
{{#each gmaps}}
  new google.maps.Marker({
    position: new google.maps.LatLng(
      {{geometry.location.lat}},
      {{geometry.location.lng}}
    ),
    map: map
  });
{{/each}}
```	
That's all (don't forget to put your own key: The key in the sample code only works for localhost request and is limited to your first test ;)

