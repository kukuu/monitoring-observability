# How to Use PM2 in This Project


- Install PM2 globally


```
npm install -g pm2

```

- Start the Conveyor Belt Simulation

``` 
pm2 start index.js --name "conveyor-belt"

```

- Check Status

```

pm2 status

```

- View Logs

```
pm2 logs

``` 

- Make It Survive Reboots

```

pm2 startup  # Sets up auto-start

```

```
pm2 save     # Saves current processes

```
