
# Browser Console Test
```
fetch('http://localhost:5000/api/simulate', {
  method: 'POST',
  headers: { 
    'Content-Type': 'application/json',
    'Accept': 'application/json'
  },
  body: JSON.stringify({ steps: 100 })
})
.then(response => {
  if (!response.ok) {
    throw new Error(`Server returned ${response.status} status`);
  }
  return response.json();
})
.then(data => {
  // Format the response data as shown in your example
  console.log('=== Simulation Results ===');
  console.log(`Products Manufactured: ${data.productsC}`);
  console.log(`Unused Component A: ${data.unusedA}`);
  console.log(`Unused Component B: ${data.unusedB}`);
  console.log(`Efficiency: ${data.efficiency}%`);
  
  console.log('\n=== Last 5 Steps ===');
  data.lastSteps.forEach((step, i) => {
    console.log(`Step ${data.stepsCompleted - 4 + i}:`, 
      `Slots: [${step.slots.join(', ')}]`,
      `State: ${step.isStopped ? 'STOPPED' : 'RUNNING'}`);
  });
  
  // If you want to display the full response
  console.log('\n=== Full Response ===');
  console.log(JSON.stringify(data, null, 2));
})
.catch(error => {
  console.error('Error:', error.message);
  console.log('Troubleshooting:');
  console.log('1. Ensure server is running: "node index.js"');
  console.log('2. Check CORS is enabled in Express: "app.use(cors())"');
  console.log('3. Try opening Chrome with: "chrome.exe --disable-web-security --user-data-dir=~/chromeTemp"');
});
```
