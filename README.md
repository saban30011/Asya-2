# Asya-2
<!DOCTYPE html>
<html lang="tr">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Asya Ülkeleri Etkileşimli Harita</title>
  <style>
    body { margin: 0; overflow: hidden; font-family: sans-serif; }
    #infoBox {
      position: absolute;
      top: 10px;
      left: 10px;
      background: rgba(255,255,255,0.9);
      padding: 15px;
      border-radius: 8px;
      max-width: 300px;
      display: none;
    }
    h2 { margin-top: 0; }
  </style>
</head>
<body>
  <div id="infoBox">
    <h2 id="countryName"></h2>
    <p><strong>Kuruluş:</strong> <span id="founding"></span></p>
    <p><strong>Başkent:</strong> <span id="capital"></span></p>
    <p><strong>Nüfus:</strong> <span id="population"></span></p>
    <p><strong>İklim:</strong> <span id="climate"></span></p>
    <p><strong>Yer Altı Kaynakları:</strong> <span id="resources"></span></p>
    <p><strong>Tarihi ve Doğal Yerler:</strong> <span id="landmarks"></span></p>
  </div>

  <script src="https://unpkg.com/three@0.152.2/build/three.min.js"></script>
  <script src="https://unpkg.com/globe.gl"></script>
  <script>
    const countriesData = {
      "Çin": {
        founding: "1 Ekim 1949",
        capital: "Pekin",
        population: "1.4 milyar",
        climate: "Karasal, Muson",
        resources: "Kömür, demir, nadir elementler",
        landmarks: "Çin Seddi, Yasak Şehir"
      },
      "Hindistan": {
        founding: "15 Ağustos 1947",
        capital: "Yeni Delhi",
        population: "1.4 milyar",
        climate: "Tropikal, muson",
        resources: "Kömür, demir cevheri, boksit",
        landmarks: "Tac Mahal, Ganj Nehri"
      },
      "Japonya": {
        founding: "11 Şubat 660 M.Ö.",
        capital: "Tokyo",
        population: "125 milyon",
        climate: "Ilıman",
        resources: "Balık, nadir mineraller (ithalat ağırlıklı)",
        landmarks: "Fuji Dağı, Kyoto Tapınakları"
      }
    };

    const world = Globe()
      (document.body)
      .globeImageUrl('//unpkg.com/three-globe/example/img/earth-dark.jpg')
      .bumpImageUrl('//unpkg.com/three-globe/example/img/earth-topology.png')
      .backgroundColor('#000')
      .pointOfView({ lat: 30, lng: 90, altitude: 2.5 }, 4000)
      .onCountryClick(country => {
        const name = country.properties.ADMIN;
        if (countriesData[name]) {
          document.getElementById('infoBox').style.display = 'block';
          document.getElementById('countryName').textContent = name;
          document.getElementById('founding').textContent = countriesData[name].founding;
          document.getElementById('capital').textContent = countriesData[name].capital;
          document.getElementById('population').textContent = countriesData[name].population;
          document.getElementById('climate').textContent = countriesData[name].climate;
          document.getElementById('resources').textContent = countriesData[name].resources;
          document.getElementById('landmarks').textContent = countriesData[name].landmarks;
        }
      });

    fetch('https://unpkg.com/world-atlas/countries-110m.json')
      .then(res => res.json())
      .then(countries => {
        const countriesGeo = window.topojson.feature(countries, countries.objects.countries).features;
        world.polygonsData(countriesGeo)
          .polygonAltitude(0.01)
          .polygonCapColor(() => `rgba(${Math.floor(Math.random()*200)},${Math.floor(Math.random()*200)},${Math.floor(Math.random()*200)},0.8)`)
          .polygonSideColor(() => 'rgba(0, 100, 0, 0.15)')
          .polygonStrokeColor(() => '#111')
          .onPolygonClick((polygon) => {
            const countryName = polygon.properties.ADMIN;
            if (countriesData[countryName]) {
              document.getElementById('infoBox').style.display = 'block';
              document.getElementById('countryName').textContent = countryName;
              document.getElementById('founding').textContent = countriesData[countryName].founding;
              document.getElementById('capital').textContent = countriesData[countryName].capital;
              document.getElementById('population').textContent = countriesData[countryName].population;
              document.getElementById('climate').textContent = countriesData[countryName].climate;
              document.getElementById('resources').textContent = countriesData[countryName].resources;
              document.getElementById('landmarks').textContent = countriesData[countryName].landmarks;
            }
          });
      });
  </script>
  <script src="https://unpkg.com/topojson@3"></script>
</body>
</html>
