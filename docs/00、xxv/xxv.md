### 01、光栅

```javascript
 var urlLists = ['./json/1.geojson', './json/2.geojson', './json/3.geojson', './json/4.geojson',
            './json/5.geojson',
            './json/6.geojson'
        ]
        urlLists.forEach(item => {
            fetch(item).then(r => r.json()).then((res) => {
                const coordinate = res.features[0].geometry.coordinates;
                const geoBoundary = CMAP.Util._createGeoBoundary({
                    coordinates: coordinate,
                    type: 'horizontal', // vertical horizontal
                    wallImage: './img/img4.png',
                    lineImage: './img/img3.png',
                    alphaImage: './img/img2.png',
                    extrudeHeight: 400,
                    lineCount: 6,
                    blending: true,
                    scrollSpeed: 1,
                    alphaSpeed: 3,
                    complete() {},
                });
                thingLayer.add(geoBoundary);
            })
        })
```

