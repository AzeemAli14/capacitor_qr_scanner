<template>
  <div>
    <button @click="startScanner">Start Scanner</button>
    <p>{{ result }}</p>
  </div>
</template>

<script>
import { BarcodeScanner } from '@capacitor-community/barcode-scanner';

export default {
  data() {
    return {
      result: '',
    };
  },
  methods: {
    async startScanner() {
      // Make sure the app has the necessary permissions
      const hasPermission = await BarcodeScanner.checkPermission({ force: true });

      if (hasPermission.camera === 'granted') {
        // Start the scanner
        BarcodeScanner.hideBackground(); // Hide the background web view (useful for mobile)
        const result = await BarcodeScanner.startScan(); // Start scanning and wait for a result

        if (result.hasContent) {
          // Handle the result
          this.result = result.content;
        }

        // Stop the scanner after the scan
        await BarcodeScanner.stopScan();
      } else {
        console.log('Camera permission not granted.');
      }
    }
  },
};
</script>
