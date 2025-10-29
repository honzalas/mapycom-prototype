Budeme dále rozvíjet tuto GitHub dokumentaci pro REST API Mapy.com
Načti si strukturu celé dokumentace, ať rozumíš projektu
------------------------------------------
Poté upravíme developer\\docs\\rest-api\\map-tiles.md 

- Dopsat více informací o mapových knihovnách, přidat jeden odstavec,  podle informací uvedených https://developer.mapy.com/rest-api-mapy-cz/map-libraries/

- příklady
Vyhod Javasript fetch a c# klient

Upravíme příklady pro Leaflet a přidáme pro Maplibre. Můžeš příklady trochu zestručnit, ale ať je z nich pochopitelný princip. A musí v nich zůstat to atribution a logo.

Leaflet příklad uprav dle tohoto kodu (včetně uvedení atribution)
// replace with your own API key
const API_KEY = 'xxx';

/*
We create the map and set its initial coordinates and zoom.
See https://leafletjs.com/reference.html#map
*/
const map = L.map('map').setView([49.8729317, 14.8981184], 16);

/*
Then we add a raster tile layer with REST API Mapy.cz tiles
See https://leafletjs.com/reference.html#tilelayer
*/
L.tileLayer(`https://api.mapy.com/v1/maptiles/basic/256/{z}/{x}/{y}?apikey=${API_KEY}`, {
  minZoom: 0,
  maxZoom: 19,
  attribution: '<a href="https://api.mapy.com/copyright" target="_blank">&copy; Seznam.cz a.s. a další</a>',
}).addTo(map);

/*
We also require you to include our logo somewhere over the map.
We create our own map control implementing a documented interface,
that shows a clickable logo.
See https://leafletjs.com/reference.html#control
*/
const LogoControl = L.Control.extend({
  options: {
    position: 'bottomleft',
  },

  onAdd: function (map) {
    const container = L.DomUtil.create('div');
    const link = L.DomUtil.create('a', '', container);

    link.setAttribute('href', 'http://mapy.com/');
    link.setAttribute('target', '_blank');
    link.innerHTML = '<img src="https://api.mapy.com/img/api/logo.svg" width="100px" />';
    L.DomEvent.disableClickPropagation(link);

    return container;
  },
});

// finally we add our LogoControl to the map
new LogoControl().addTo(map);

Přidej Malibre Example
// replace with your own API key
const API_KEY = 'xxx';

/*
We create a map with initial coordinates zoom, a raster tile source and a layer using that source.
See https://maplibre.org/maplibre-gl-js-docs/example/map-tiles/
See https://maplibre.org/maplibre-gl-js-docs/style-spec/sources/#raster
See https://maplibre.org/maplibre-gl-js-docs/style-spec/layers/
*/
const map = new maplibregl.Map({
	container: 'map',
	center: [14.8981184, 49.8729317],
	zoom: 15,
	style: {
		version: 8,
		sources: {
			'basic-tiles': {
				type: 'raster',
				url: `https://api.mapy.com/v1/maptiles/basic/tiles.json?apikey=${API_KEY}`,
				tileSize: 256,
			},
		},
		layers: [{
			id: 'tiles',
			type: 'raster',
			source: 'basic-tiles',
		}],
	},
});

/*
We also require you to include our logo somewhere over the map.
We create our own map control implementing a documented interface,
that shows a clickable logo.
See https://maplibre.org/maplibre-gl-js-docs/api/markers/#icontrol
*/
class LogoControl {
	onAdd(map) {
		this._map = map;
		this._container = document.createElement('div');
		this._container.className = 'maplibregl-ctrl';
		this._container.innerHTML = '<a href="http://mapy.com/" target="_blank"><img  width="100px" src="https://api.mapy.com/img/api/logo.svg" ></a>';

		return this._container;
	}
 
	onRemove() {
		this._container.parentNode.removeChild(this._container);
		this._map = undefined;
	}
}

// finally we add our LogoControl to the map
map.addControl(new LogoControl(), 'bottom-left');

------------------------------------------
Upravíme Geocoding MD developer\docs\rest-api\geocoding.md

Info o endpointech Geocode a Suggest uváděj za sebou, jsou hodně příbuzné. Oboje patří pod forward geocoding.

Přidej tam tento popis, který jasněji vysvětlí rozdíly mezi funkcemi
Geokódování – Vyhledává entity (adresy) na základě textového dotazu. Získá souřadnice a další informace (např. okolní regionální strukturu)
Našeptávání – funguje podobně jako geokódování, ale zohledňuje neúplné dotazy, takže jej lze použít k navrhování odpovídajících entit, když uživatel píše dotaz na umístění.
Reverzní geokódování – vrací regionální entity na základě souřadnice (při kliknutí do mapy vrací adresy v daném bodě)

Ponech pouze CURL příklady a Geocode v c# dále přidej

Příklad pro autocomplete(suggest), pomocí autocomplete.js - zdůrazni že jde o externí komponentu.
<!DOCTYPE html>
<html lang="en">

<head>
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/@tarekraafat/autocomplete.js@10.2.7/dist/css/autoComplete.02.min.css">
</head>

<body>
    <div class="autoComplete_wrapper">
        <input id="autoComplete" type="search" dir="ltr" spellcheck=false autocorrect="off" autocomplete="off" autocapitalize="off">
    </div>

    <script src="https://cdn.jsdelivr.net/npm/@tarekraafat/autocomplete.js@10.2.7/dist/autoComplete.min.js"></script>
</body>

</html>
skript
const API_KEY = 'eyJpIjoyNTcsImMiOjE2Njc0ODU2MjN9.c_UlvdpHGTI_Jb-TNMYlDYuIkCLJaUpi911RdlwPsAY';
const inputElem = document.querySelector("#autoComplete");
// cache - [key: query] = suggest items
const queryCache = {};
// get items by query
const getItems = async(query) => {
	if (queryCache[query]) {
  	return queryCache[query];
  }
  
	try {
    // you need to use your own api key!
  	const fetchData = await fetch(`https://api.mapy.cz/v1/suggest?lang=cs&limit=5&type=regional.address&apikey=${API_KEY}&query=${query}`);
    const jsonData = await fetchData.json();
    // map values to { value, data }
    const items = jsonData.items.map(item => ({
      value: item.name,
      data: item,
    }));
    
    // save to cache
    queryCache[query] = items;
    
    return items;
  } catch (exc) {
  	return [];
  }
};

const autoCompleteJS = new autoComplete({
	selector: () => inputElem,
	placeHolder: "Enter your address...",
  searchEngine: (query, record) => `<mark>${record}</mark>`,
	data: {
    keys: ["value"],
		src: async(query) => {
    	// get items for current query
      const items = await getItems(query);
      
      // cache hit? - there is a problem, because this provider needs to get items
      // for each query and cannot handle different timeouts for different query.
      // if previous query was completed - it's already in the cache, and some
      // old query is completed, we test it againts current query and returns correct items.
      if (queryCache[inputElem.value]) {
      	return queryCache[inputElem.value];
      }
      
      return items;
		},
		cache: false,
	},
	resultItem: {
  	element: (item, data) => {
    	const itemData = data.value.data;
    	const desc = document.createElement("div");
      
      desc.style = "overflow: hidden; white-space: nowrap; text-overflow: ellipsis;";
      desc.innerHTML = `${itemData.label}, ${itemData.location}`;
      item.append(
      	desc,
      );
    },
		highlight: true
	},
	resultsList: {
 		 element: (list, data) => {
     	list.style.maxHeight = "max-content";
    	list.style.overflow = "hidden";
    
       if (!data.results.length) {
         const message = document.createElement("div");
         
         message.setAttribute("class", "no_result");
         message.style = "padding: 5px";
         message.innerHTML = `<span>Found No Results for "${data.query}"</span>`;
         list.prepend(message);
       } else {
       	const logoHolder = document.createElement("div");
        const text = document.createElement("span");
        const img = new Image();
        
        logoHolder.style = "padding: 5px; display: flex; align-items: center; justify-content: end; gap: 5px; font-size: 12px;";
        text.textContent = "Powered by";
        img.src = "https://api.mapy.cz/img/api/logo-small.svg";
        img.style = "width: 60px";
        logoHolder.append(text, img);
       	list.append(logoHolder);
       }
     },
		noResults: true,
	},
});
inputElem.addEventListener("selection", event => {
    // "event.detail" carries the autoComplete.js "feedback" object
    // saved data from line 16 (mapping)
    const origData = event.detail.selection.value.data;
    // data to debug
    console.log(origData);
    inputElem.value = origData.name;
});