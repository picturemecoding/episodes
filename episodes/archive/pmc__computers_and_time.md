# Computers and Time

**Episode Type:** Topic Episode
**Hosts:** Mike + co-host
**Date:** 2025-01-01

---

## Episode Summary

---

(We could actually break this up into multiple episodes )

Erik Music: Good Looks *Lived Here For a While* (“If It’s Gone” “I hope you find the courage to tell your parents what they did, when they left you all alone locked up in that room when you were a kid.” I like the song “Day of Judgment” a lot too: “we’re all helpless with the Wheel of Fortune still spinning.”)

Mike Music: Nilufer Yanya, *My Method Actor*.

## Discussion Sections

1. Anecdotes (Randy’s email), “dates don’t have timezones” argument
2. Astronomical time, UTC, leap seconds
3. Fallacies Programmers believe about time
4. Lamport Clocks and relativity

## 1. Randy’s Email, Anecdotes about time

- Randy’s email:

What have you been listening to!? I'm relying on you guys to discover new music.

I also take minor issue with your treatment of story points. My understanding is that the reason story points exist as an abstraction from directly estimating time is that [empirical evidence](https://www.duo.uio.no/handle/10852/10127) suggests that human beings produce more consistent and accurate estimations when doing relative estimation. I think it's valid to convert from story points to time in the aggregate, though not to do the reverse. For example, in my org we estimate using story points but capture our effort (how long it took to resolve a ticket) using hours. The historic data then gives us a way to convert from story points to hours during project planning when we're estimating very large projects. Of course we're still considering this a rough approximation and adding a healthy buffer, but it's been pretty accurate for the past few years. It helps if you keep the projects as small as possible.

Obviously, software development estimation is still a black art and I wouldn't swear by any of this, but it's a functioning mechanic for now. Hope you guys are having fun. I love the podcast.

[MENTION THE EMAIL ADDRESS AGAIN! podcast@picturemecoding.com]

- Randy’s “empirical evidence” link is for “Empirical research on relative and absolute effort estimation in software development projects”: <https://www.duo.uio.no/handle/10852/10127>
- Erik’s commentary: points and time, abstraction is useful. Engineering manager vs IC (we can no longer sympathize with each other like in Mystic River or Heat)
- That one time at work that someone asked me to produce a date in a payload relating to when the payload was produced and I said okay, I’ll use UTC and they replied “dates don’t have timezones” and then I tried (and failed) to explain that we were sometimes producing these dates in a server on the west coast at the end of the day and we were sending them to servers in other parts of the US. If I use local time to generate the date, it’ll probably just “work out”, but UTC seems more reliable?

## 2. Astronomical time, UTC, leap seconds

Check out this section from page 251 of the textbook [Distributed Systems](https://www.distributed-systems.net/) by Maarten van Steen:

Since the invention of mechanical clocks in the 17th century, time has been measured astronomically. Every day, the sun appears to rise on the eastern horizon, then climbs to a maximum height in the sky, and finally sinks in the west. The event of the sun’s reaching its highest apparent point in the sky is called the transit of the sun. This event occurs at about noon each day. The interval between two consecutive transits of the sun is called the solar day. Since there are 24 hours in a day, each containing 3600 seconds, the solar second is defined as exactly 1/86400th of a solar day. [p 251Distributed Systems textbook]

In the 1940s, it was established that the period of the earth’s rotation is not constant. The earth is slowing down due to tidal friction and atmospheric drag. Based on studies of growth patterns in ancient coral, geologists now believe that 300 million years ago there were about 400 days per year. The length of the year (the time for one trip around the sun) is not thought to have changed; the day has simply become longer. In addition to this long-term trend, short-term variations in the length of the day also occur, probably caused by turbulence deep in the earth’s core of molten iron. These revelations lead astronomers to compute the length of the day by measuring a large number of days and taking the average before dividing by 86,400. The resulting quantity was called the mean solar second.

— SIDEBAR HERE but WHAT?! —

There’s a group dedicated to measuring and reporting the earth’s rotation: “The International Earth Rotation and Reference Systems Service (IERS) is the body responsible for maintaining global time and reference frame standards, notably through its Earth Orientation Parameter (EOP) and International Celestial Reference System (ICRS) groups.” ([wikipedia](https://en.wikipedia.org/wiki/International_Earth_Rotation_and_Reference_Systems_Service))

“Tidal deceleration rates have varied over the history of the Earth-Moon system. Analysis of layering in fossil mollusc shells from 70 million years ago, in the Late Cretaceous period, shows that there were 372 days a year, and thus that the day was about 23.5 hours long then.[20][21] Based on geological studies of tidal rhythmites, the day was 21.9±0.4 hours long 620 million years ago and there were 13.1±0.1 synodic months/year and 400±7 solar days/year.” ([wikipedia](https://en.wikipedia.org/wiki/%CE%94T_%28timekeeping%29)) [It’s estimated that over 1 billion years ago the day was about 19 hours.]

Distributed Systems book continued…

With the invention of the atomic clock in 1948, it became possible to measure time much more accurately, and independent of the wiggling and wobbling of the earth, by counting transitions of the cesium 133 atom. The physicists took over the job of timekeeping from the astronomers and defined the second to be the time it takes the cesium 133 atom to make exactly 9,192,631,770 transitions. The choice of 9,192,631,770 was made to make the atomic second equal to the mean solar second in the year of its introduction. Currently, several laboratories around the world have cesium 133 clocks. Periodically, each laboratory tells the Bureau International de l’Heure (BIH) in Paris how many times its clock has ticked. The BIH averages these to produce International Atomic Time, which is abbreviated to TAI. Thus TAI is just the mean number of ticks of the cesium 133 clocks since midnight on Jan. 1, 1958 (the beginning of time) divided by 9,192,631,770.

Although TAI is highly stable and available to anyone who wants to go to the trouble of buying a cesium clock, there is a serious problem with it; 86,400 TAI seconds is now about 3 msec less than a mean solar day (because the mean solar day is getting longer all the time). Using TAI for keeping time would mean that over the course of the years, noon would get earlier and earlier, until it would eventually occur in the wee hours of the morning. People might notice this and we could have the same kind of situation as occurred in 1582 when Pope Gregory XIII decreed that 10 days be omitted from the calendar. This event caused riots in the streets because landlords demanded a full month’s rent and bankers a full month’s interest, while employers refused to pay workers for the 10 days they did not work, to mention only a few of the conflicts. The Protestant countries, as a matter of principle, refused to have anything to do with papal decrees and did not accept the Gregorian calendar for 170 years.

BIH solves the problem by introducing leap seconds whenever the discrepancy between TAI and solar time grows to 800 msec. The use of leap seconds is illustrated in Figure 5.3. This correction gives rise to a time system based on constant TAI seconds but which stays in phase with the apparent motion of the sun. This time system is known as Coordinated Universal Time abbreviated to UTC.

There is a plan to [abandon the leap second by 2035](https://en.wikipedia.org/wiki/Leap_second)!

The leap second was introduced in 1972. Since then, 27 leap seconds have been added to UTC, with the most recent occurring on December 31, 2016.[1] All have so far been positive leap seconds, adding a second to a UTC day; while it is possible for a negative leap second to be needed, one has not happened yet.

Because the Earth's rotational speed varies in response to climatic and geological events,[2] UTC leap seconds are irregularly spaced and unpredictable. Insertion of each UTC leap second is usually decided about six months in advance by the International Earth Rotation and Reference Systems Service (IERS), to ensure that the difference between the UTC and UT1 readings will never exceed 0.9 seconds.[3][4]

This practice has proven disruptive, particularly in the twenty-first century and especially in services that depend on precise timestamping or time-critical process control. And since not all computers are adjusted by leap-second, they will display times differing from those that have been adjusted.[5] After many years of discussions by different standards bodies, in November 2022, at the 27th General Conference on Weights and Measures, it was decided to abandon the leap second by or before 2035.[6][7]

Back to the distributed systems book…

Most electric power companies synchronize the timing of their 60-Hz or 50-Hz clocks to UTC, so when BIH announces a leap second, the power companies raise their frequency to 61 Hz or 51 Hz for 60 or 50 sec, to advance all the clocks in their distribution area. Since 1 sec is a noticeable interval for a computer, an operating system that needs to keep accurate time over a period of years must have special software to account for leap seconds as they are announced (unless they use the power line for time, which is usually too crude). The total number of leap seconds introduced into UTC so far is about 30.

![](data:image/png;base64...)

The basis for keeping global time is called Coordinated Universal Time, but is abbreviated as UTC. UTC is the basis of all modern civil timekeeping and is a worldwide standard. To provide UTC to people who need precise time, some 40 shortwave radio stations around the world broadcast a short pulse at the start of each UTC second. The accuracy of these stations is about ± 1 msec, but due to random atmospheric fluctuations that can affect the length of the signal path, in practice the accuracy is no better than ± 10 milliseconds.

Several earth satellites also offer a UTC service. The Geostationary Operational Environment Satellite can provide UTC accurately to 0.5 msec, and some other satellites do even better. By combining receptions from several satellites, ground timeservers can be built, offering an accuracy of 50 nanoseconds. UTC receivers are commercially available, and many computers are equipped with one.

Systems that care about timestamps

- Cell phone towers
- Distributed logging
- Some databases

# Computers and Time, Episode 2

A second episode on time, this time focusing on how time comes up in software and why this frequently turns out to be a nightmare.

Erik Music: Graveyard *Hisingen Blues* - from 2011 but new to me. Sounds like a record from 1969, which normally would turn me off, but when it comes on I feel like, “wow, I really need this in my life.” It’s a bluesy, led zeppelin thing, really good.

Mike Music: Charlie Overman - *Charlie Overman*

I have a weird relationship with country music. I spent most of my youth on a farm in Indiana. My dad’s F-150 only had AM radio, and the only channels we could reliably get were WOWO, which was farm reports and Top 40, and the country station. I listened to a lot of country music, and a lot of it i really loved, and still do. And not just high falutin’ alt-country or left-leaning folkies. One of my favorites as a kid was Moe Bandy and Joe Stampy’s song “Just Good Ol’ Boys”, which has the memorable line “I hocked my wife's diamond ring last June, Bought me an outboard Evinrude”. On the other hand, when i listen to “Hot Country” on Spotify, i’ll skip many songs. It seems insincere. It seems like warmed over pop rock, with singers affecting a little extra twang. Charle Overman is country music, with country themes, but doesn’t make me feel stupid.

Erik to introduce: woke up the other day when my alarm went off… Also, saw a Nova episode about quantum mechanics…

I feel like every time someone at work says, “In your program, if we can’t do X, can we schedule it for a month in the future…?” and I give them a look that says, “that’s kinda hard, boss” and then I start asking a whole bunch of questions, it becomes another one of those moments where non-programmers are looking at us like, “why are you people so weird?” “Why is it so hard for you to do something that’s *so easy*: I want something to happen in a month. How is that so difficult?”

## How Do Programmers Use Time

- Timestamp the creation and modification of records
  + For the benefit of humans
    - Auditing
  + Querying/Data Mining
    - Time can be used as a filter
    - Time can be used a dimension for aggregation (by day, by month)
  + To trigger certain actions
    - A record older than N days gets archived if there’s no activity.
    - Or, say, a reminder is sent
  + Scheduling
    - Run job X at midnight every weekday
    - Run a process for 100ms or until it sleeps
  + Ordering
    - Which modification was made first?
    - Changelogs
  + Measurement
    - GPS
    - ICMP/ping

Some implications of these applications:

1. The time on your computer is correct(ish)
2. The time on your computer is consistent with the time on other computers
3. The time zone you’re specifying is the same as the timestamp on the data
4. The value for time is precise enough to differentiate events
5. There is, somewhere, a “correct” time on which everyone agrees

## 1. Time Sync Protocols

- NTP
- PTP
- Sundial

David L. Mills of the University of Delaware designed NTP. He died in January of this year, in fact. They called him the internet's "Father Time".

From [wikipedia](https://en.wikipedia.org/wiki/Network_Time_Protocol):

NTP is intended to synchronize all participating computers to within a few milliseconds of Coordinated Universal Time (UTC). It uses the intersection algorithm, a modified version of Marzullo's algorithm, to select accurate time servers and is designed to mitigate the effects of variable network latency. NTP can usually maintain time to within tens of milliseconds over the public Internet, and can achieve better than one millisecond accuracy in local area networks under ideal conditions. Asymmetric routes and network congestion can cause errors of 100 ms or more.

NTP:

- Client-server or peer-to-peer
- UDP on port number 123
- Timestamps: client-sent, server-received, server-sent, client-received

![](data:image/png;base64...)

Possibly discuss Cesium clocks in connection with 5

This has the best explanation i’ve seen of Cesium clocks: [How an Atomic Clock Really Works: Inside the HP 5061A Cesium Clock](https://www.youtube.com/watch?v=eOti3kKWX-c)

Cesium:

- [Keeping Time at NIST](https://www.nist.gov/blogs/taking-measure/keeping-time-nist)
- [the Nova episode](https://www.youtube.com/watch?v=t06aTX9jM34)

Talk about Time Zones in connection with 3 (see above), then TZ fallacies.

## 2. Time Zones and Why We Hate Them

Apparently time zones were created by some dude named Sanford Fleming, in order to help keep the trains running on time. He devised the plan to divide the world up into 24 time zones based on 15-degree chunks of latitude. I guess the idea of having one global time for everyone was unacceptable because people didn’t want the sun to come up at 7pm, or whatever. Unfortunately, dividing the planet up into equal orange sections was about as clean as dividing up a chicken into a 20 piece McNuggets, so now the whole system is a calamity.

Fun fact: since time zones are based on longitude (but some timezones share a longitude), they get closer as you move north and south. Apparently, scientists at the poles just use UTC.

A common scenario: a record is created in a database in the Pacific Time Zone on a hot summer day in San Diego.

- A person viewing the record in Utah will expect to see the creation time on the record in the user interface in local time (MDT).
- Same for a person in Arizona, which does not observe Daylight Savings Time
- Somebody in Gary, Indiana makes several edits to the record (Gary is on central time, while most of Indiana is Eastern)
- Creation of the record triggers creation of a second related record, possibly within milliseconds of the first.
- Various external systems get notifications of creation and modification of records and try to keep their own version of the data in a consistent way

Although you can usually store information with a timezone and then convert to local time, I think most systems use the method of using UTC internally and using local time mostly for display. This leaves some problems though:

- If no TZ is specified, do you assume it’s local time or UTC?
- Local client conversions will only work if the local TZ is correct for the client computer.
- UTC -> local time conversions are still tricky (see the TZ fallacies)

References:

- (2012-06-19) [Falsehoods Programmers Believe About Time](https://infiniteundo.com/post/25326999628/falsehoods-programmers-believe-about-time)
- (2012-06-20) ‘[More falsehoods programmers believe about time; “wisdom of the crowd” edition](https://infiniteundo.com/post/25509354022/more-falsehoods-programmers-believe-about-time)’
- (both of the above in a bulleted list here: <https://gist.github.com/timvisee/fcda9bbdff88d45cc9061606b4b923ca>)
- Falsehoods programmers believe about timezones: <https://www.zainrizvi.io/blog/falsehoods-programmers-believe-about-time-zones/>
- Time can’t go backwards! ([Cloudflare leap second bug](https://blog.cloudflare.com/how-and-why-the-leap-second-affected-cloudflare-dns/))

Of the fallacies I want to talk about:

- “The system clock will never be set to a time that is in the distant past or the far future.”
- “One minute on the system clock has exactly the same duration as one minute on any other clock”
- “A timestamp represents the time that an event actually occurred.”
- “Non leap years will never contain a leap day.”
- “The day before Saturday is always Friday.”
- “Time always goes forwards.”
- “My software is only used internally/locally, so I don’t have to worry about timezones.” (this was the conversation I had with the engineer where he said “dates don’t have timezones”). I think the problem with his statement is that it’s like there needs to be a grounding for the meaning of this timestamp: what will it mean when it’s looked at or used?
- Time Zones: [a country’s time zone never changes](https://www.zainrizvi.io/blog/falsehoods-programmers-believe-about-time-zones/#misconception-10-a-country-s-time-zone-never-changes)…
- UTF spans go from -12 to +12. “UTC offsets span from -12 to +14. Yeah, +14. That's gives you 27 hours UTC can be offset by (don't forget the zero offset)”
- “Every UTC offset corresponds to exactly one time zone” (gives example of 10 timezones which are all +5)
- “India standard time is five and a half hours off of UTC… Nepal likes to be at the 45 minute UTC offset.” “…Because they really want their mountain to have the sun right above it at noon.”

## 3. Lamport Clocks and relativity

Three types of time:

- Wall-clock time
- Monotonic clocks
- Logical clocks

Lamport says our programs are subject to relativity?!