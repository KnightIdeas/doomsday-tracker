<template>
  <div>
    <h2>JSONBin Data Test</h2>
    <pre>{{ testData }}</pre>
  </div>
</template>

<script>
export default {
    data() {
        return {
            testData: null
        }
    },
    created() {
        fetch(`https://api.jsonbin.io/v3/b/${import.meta.env.VITE_JSONBIN_ID}/latest`, {
            method: 'GET',
            headers: {
                'X-Master-Key': import.meta.env.VITE_JSONBIN_X_MASTER_KEY,
                "X-Access-Key": import.meta.env.VITE_JSONBIN_X_ACCESS_KEY
            }
        })
        .then(response => response.json())
        .then(data => {
            this.testData = data.record
            console.log('Data fetched successfully', data)
            // Output the environment variables
            console.log(import.meta.env.VITE_JSONBIN_ID)
            console.log('X Master Key', import.meta.env.VITE_JSONBIN_X_MASTER_KEY)
            console.log('X Access Key', import.meta.env.VITE_JSONBIN_X_ACCESS_KEY)
        })
        .catch(error => {
            console.error('There was an error!', error)
        })
    }
}
</script>

<style>

</style>