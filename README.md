# DD-Track

This is a small dashboard for tracking tasks completed around the house. Data is input through [Habitica](https://habitica.com/).

> Habitica gamifies your life by giving you in-game rewards for your real-life tasks. The better you are at accomplishing real-life tasks, the more you progress in the game.

My wife and I check off specific tasks every time we mop the floor, cook our lunch or whatever. Unfortunately, Habitica doesn’t store any history, which is the motivation behind this project. The dashboard provides a history of completed tasks, statistics, a calendar view (desktop only), and various filters—so I can prove I do more dishes than she does! 😜 (spoiler: I failed)

## ⚙️ Tech used

Created with [vuetify create](https://github.com/vuetifyjs/create)
Backend is basically a node.js [webhook](https://habitica.fandom.com/wiki/Webhooks) that listens to Habitica's task completion event. It then stores this data into a postgres database. Something like this:

```
  app.post("/task-webhook", function (req, res) {
    // reply habitica server asap
    res.status(200).json({});

    const webhookEvent = req.body;

    // extract task data from webhookEvent
    // insert data into db

  });
```

## 📑 License

Feel free to use or modify this version as you see fit! ¯\\\_(ツ)\_/¯
