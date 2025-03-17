<template>
    <div>
      <h2>JSONBin Data Test</h2>
      <pre>{{ testData }}</pre>
  
      <!-- Quick test button -->
      <button @click="testAddPrediction">Add Test Prediction</button>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        predictions: []
      };
    },
    
    mounted() { // use 'mounted' for initial fetching
      fetch(`https://api.jsonbin.io/v3/b/${import.meta.env.VITE_JSONBIN_ID}/latest`, {
        method: 'GET',
        headers: {
          'X-Master-Key': import.meta.env.VITE_JSONBIN_X_MASTER_KEY,
          'X-Access-Key': import.meta.env.VITE_JSONBIN_X_ACCESS_KEY
        }
      })
      .then(res => res.json())
      .then(responseData => {
        this.predictions = responseData.record.predictions;
        console.log("Fetched predictions successfully", this.predictions);
      })
      .catch(err => console.error(err));
    },
    
    methods: {  // ✅ Correct syntax for Vue methods
      addPrediction(newPrediction) {
        fetch(`https://api.jsonbin.io/v3/b/${import.meta.env.VITE_JSONBIN_ID}/latest`, {
          method: 'GET',
          headers: {
            'X-Master-Key': import.meta.env.VITE_JSONBIN_X_MASTER_KEY,
            "X-Access-Key": import.meta.env.VITE_JSONBIN_X_ACCESS_KEY
          }
        })
        .then(response => response.json())
        .then(responseData => {
          let predictions = responseData.record.predictions || [];
          predictions.push(newPrediction);
  
          return fetch(`https://api.jsonbin.io/v3/b/${import.meta.env.VITE_JSONBIN_ID}`, {
            method: 'PUT',
            headers: {
              'Content-Type': 'application/json',
              'X-Master-Key': import.meta.env.VITE_JSONBIN_X_MASTER_KEY,
              "X-Access-Key": import.meta.env.VITE_JSONBIN_X_ACCESS_KEY
            },
            body: JSON.stringify({ record: { predictions } }) // ✅ Use 'record'
          });
        })
        .then(() => console.log('Prediction added successfully'))
        .catch(error => console.error('Error adding prediction:', error));
      },
      
      // For easy testing:
      testAddPrediction() {
        const samplePrediction = {
          id: Date.now().toString(),
          event: 'Test Event',
          description: 'This is a test prediction.',
          date: '2050-01-01',
          disaster_type: 'test',
          author: 'Testing Bot',
          source: 'https://example.com',
          status: 'impending'
        };
        this.addPrediction(samplePrediction);
      }
    }
  };
  </script>
  