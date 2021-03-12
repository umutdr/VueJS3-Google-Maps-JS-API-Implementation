<script>
import { POINT_MARKER_ICON_CONFIG } from '@/constants/mapSettings';

export default {
  data: () => ({
    marker: null,
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
        draggable: true,
        animation: google.maps.Animation.DROP,
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
