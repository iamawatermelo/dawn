# dawn

Simple sunrise alarm clock.

## Get started

Visit the website: <https://dawn.srh.dog/>  
Or, use `npm run dev` to develop locally.

To configure Dawn, use these query parameters:

- h: hour to start animating sunrise (default: 6)
- m: minute to start animating sunrise (default: 0)
- t: how long the sunrise lasts (default: 1 hour)
- h2, m2, t2: the same, but for sunset (default: 1 hour from 20:00)

For example, to wake you up over 30 minutes from 7am, the URL would be
<https://dawn.srh.dog/?h=7&t=1800>.