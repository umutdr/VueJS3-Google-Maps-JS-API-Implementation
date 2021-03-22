<script>
import { POINT_MARKER_ICON_CONFIG } from '@/constants/mapSettings';

export default {
  data: () => ({
    marker: null,
    iconBaseUri: 'http://earth.google.com/images/kml-icons/track-directional/',
  }),
  props: {
    lat: { type: Number, required: true },
    lng: { type: Number, required: true },
  },
  mounted() {
    this.$parent.getMap((map) => {
      this.marker = new window.google.maps.Marker({
        position: { lat: this.lat, lng: this.lng },
        map: map,
        animation: google.maps.Animation.DROP,
        icon: this.iconBaseUri + 'track-0.png',
      });

      this.marker.addListener('click', () => {
        console.log('marker clicked');
      });
    });
  },
  beforeUnmount() {
    this.marker.setMap(null);
    window.google.maps.event.clearInstanceListeners(this.marker);
  },
  render() {
    return null;
  },
};
</script>
