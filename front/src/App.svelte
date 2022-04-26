<script>
  import OpenSeadragon from "openseadragon"
  import { onMount } from "svelte"
  
  var dziFilesUrl = '//openseadragon.github.io/example-images/duomo/duomo_files/';
  // Change this to the contents of the .dzi file from your server. 
  var dziData = '<?xml version="1.0" encoding="utf-8"?><Image TileSize="254" Overlap="1" Format="jpg" xmlns="http://schemas.microsoft.com/deepzoom/2008"><Size Width="13920" Height="10200"/></Image>';

  // This converts the XML into a DZI tile source specification object that OpenSeadragon understands. 
  var tileSourceFromData = function(data, filesUrl) {
    /* var $xml = ($.parseXML(data));
    var $image = $xml.find('Image');
    var $size = $xml.find('Size'); */

    var dzi = {
      Image: {
        xmlns: "http://schemas.microsoft.com/deepzoom/2008",
        Url: filesUrl,
        Format: "jpg",
        Overlap: "1",
        TileSize: "254",
        Size: {
          Height: "10200",
          Width: "13920"
        }
      }
    };  
    
    console.log(dzi);
    return dzi;
  };

  onMount(() =>{
    let viewer = new OpenSeadragon({
        id: "view",
        prefixUrl: '//openseadragon.github.io/openseadragon/images/',
        tileSources: tileSourceFromData(dziData, dziFilesUrl),
        showNavigator: true,
        showRotationControl: true,
        animationTime: 0.5,
        blendTime: 0.1,
        constrainDuringPan: true,
        maxZoomPixelRatio: 2,
        minZoomLevel: 1,
        visibilityRatio: 1,
        zoomPerScroll: 2,
        timeout: 120000,
    });
    viewer.addHandler("open", function() {
        // To improve load times, ignore the lowest-resolution Deep Zoom
        // levels.  This is a hack: we can't configure the minLevel via
        // OpenSeadragon configuration options when the viewer is created
        // from DZI XML.
        viewer.source.minLevel = 8;
    });
  })

</script>
<h1> Svelte OpenSeadragon </h1>
<div id=view> </div>

<style>
  #view {
    position: fixed;
    left: 10%;
    top: 20%;
    width: 80%;
    height: 70%;
  }
</style>
