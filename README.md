# marker cluster demo

![](./demo.gif)

```js
document.addEventListener("deviceready", function () {

    var mapDiv = document.getElementById("map_canvas");
    var options = {
        'camera': {
            'target': data[0].position,
            'zoom': 3
        }
    };
    var map = plugin.google.maps.Map.getMap(mapDiv, options);
    map.on(plugin.google.maps.event.MAP_READY, onMapReady);
});


function onMapReady() {
    var map = this;

    var label = document.getElementById("label");

    //------------------------------------------------------
    // Create a marker cluster.
    // Providing all locations at the creating is the best.
    //------------------------------------------------------
    map.addMarkerCluster({
      //debug: true,
      //maxZoomLevel: 5,
      markers: data,
      icons: [
          {min: 2, max: 100, url: "./img/blue.png", anchor: {x: 16, y: 16}},
          {min: 100, max: 1000, url: "./img/yellow.png", anchor: {x: 16, y: 16}},
          {min: 1000, max: 2000, url: "./img/purple.png", anchor: {x: 24, y: 24}},
          {min: 2000, url: "./img/red.png",anchor: {x: 32,y: 32}},
      ]
    }, function (markerCluster) {

        //-----------------------------------------------------------------------
        // Display the resolution (in order to understand the marker cluster)
        //-----------------------------------------------------------------------
        markerCluster.on("resolution_changed", function (prev, newResolution) {
            var self = this;
            label.innerHTML = "<b>zoom = " + self.get("zoom") + ", resolution = " + self.get("resolution") + "</b>";
        });
        markerCluster.trigger("resolution_changed");



        //----------------------------------------------------------------------
        // Remove the marker cluster
        // (Don't remove/add repeatedly. This is really bad performance)
        //----------------------------------------------------------------------
        var removeBtn = document.getElementById("removeClusterBtn");
        removeBtn.addEventListener("click", function() {
          markerCluster.remove();
        }, {
          once: true
        });

        //------------------------------------
        // If you tap on a marker,
        // you can get the marker instnace.
        // Then you can do what ever you want.
        //------------------------------------
        var htmlInfoWnd = new plugin.google.maps.HtmlInfoWindow();
        markerCluster.on(plugin.google.maps.event.MARKER_CLICK, function (position, marker) {
          var html = [
            "<div style='width:250px;min-height:100px'>",
            "<img src='img/starbucks_logo.gif' align='right'>",
            "<strong>" + (marker.get("title") || marker.get("name")) + "</strong>"
          ];
          if (marker.get("address")) {
            html.push("<div style='font-size:0.8em;'>" + marker.get("address") + "</div>");
          }
          if (marker.get("phone")) {
            html.push("<a href='tel:" + marker.get("phone") + "' style='font-size:0.8em;color:blue;'>Tel: " + marker.get("phone") + "</div>");
          }
          html.push("</div>");
          htmlInfoWnd.setContent(html.join(""));
          htmlInfoWnd.open(marker);
        });


    });

}
```
