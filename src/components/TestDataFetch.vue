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
    method: {
        addData() {
            // Add data to the JSONBIN.io database with the method of PUT
            fetch(`https://api.jsonbin.io/v3/b/${import.meta.env.VITE_JSONBIN_ID}`, {
                method: 'PUT',
                headers: {
                    'Content-Type': 'application/json',
                    'X-Master-Key': import.meta.env.VITE_JSONBIN_X_MASTER_KEY,
                    "X-Access-Key": import.meta.env.VITE_JSONBIN_X_ACCESS_KEY
                },
                body: JSON.stringify({
                    record: {

                    prediction:
                    {
                    id: Date.now().toString(), // ✅ unique ID
                    event: 'Test Event',
                    description: 'This is a test prediction 2.',
                    date: '2050-01-01',
                    disaster_type: 'test',
                    author: 'Test Author',
                    source: 'https://example.com',
                    status: 'impending',
                    }
                }
                })
            })
                .then(response => response.json())
                .then(data => {
                    console.log('Data added successfully', data)
                })
                .catch(error => {
                    console.error('There was an error!', error)
                })
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
            // Add data to the JSONBIN.io database
            addData()  
    }
}
</script>

<style></style>