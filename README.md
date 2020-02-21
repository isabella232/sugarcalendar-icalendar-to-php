# iCalendar to PHP

Parse an iCalendar URI, with PHP, into some useful format.


## Disclaimer

This project is new. Structural & architectural changes may occur without warning.

This disclaimer will be removed when stability has been achieved.

## What is iCalendar?

iCalendar is a widely popular specification that standardizes what an “Event” is and provides directives on how software should expect to interact with them.

It is what Outlook/Office, and Apple Calendar, and Google and others all use for their calendar applications. It’s also what many WordPress Events plugins use, in one way or another, to allow for visitors to subscribe to a feed of events directly from their sites.

## What does this do?

This PHP class is a generic way, without any dependencies, to read an entire iCalendar stream into PHP.

It accepts any URI (web or local), pulls it into memory, does some basic error correction, loops through the file line-by-line and splits it apart into a big array of iCalendar Components, Properties, Parameters, and Values, so that _something_ can be done with it all later.

It has built-in support for WordPress, but (currently) does not consider it a requirement.

## Why did you build this?

We built this for Sugar Calendar to be able to reliably interoperate with any other calendar application.

We did a huge amount of research looking for anything like this already that we could use, and came up totally empty. Many other libraries do exist that bridge iCalendar and PHP in some way, but not as simply or effectively as we were able to achieve here.

## How's it work?

In a WordPress plugin, you can test this quickly with something like...

```
add_action( 'init', function() {

	// Pick a URI
	$uri = 'https://data.nba.net/prod/teams/schedules/2019/rockets_schedule.ics';

	// Load it
	$ical = new Sugar_Calendar\Utilities\iCalendar\ToArray( $uri );

	// Get all data
	$ical_data = $ical->get_all_data();

	// Dump it out
	var_dump( $ical_data ); die;
} );
```

The iCalendar specification defines a set of Components, Properties, Parameters, and Values.

The `VCALENDAR` Component looks something like...

```
array (size=3)
  'VCALENDAR' => 
    array (size=9)
      'VERSION' => 
        array (size=1)
          'formatted' => string '2.0' (length=3)
      'METHOD' => 
        array (size=1)
          'formatted' => string 'PUBLISH' (length=7)
      'X-WR-TIMEZONE' => 
        array (size=1)
          'formatted' => string 'America/New_York' (length=16)
      'PRODID' => 
        array (size=1)
          'formatted' => string '-//Apple Inc.//iCal 3.0//EN' (length=27)
      'CALSCALE' => 
        array (size=1)
          'formatted' => string 'GREGORIAN' (length=9)
      'X-WR-CALNAME' => 
        array (size=1)
          'formatted' => string 'Rockets Schedule 2019-2020' (length=26)
      'X-WR-CALDESC' => 
        array (size=1)
          'formatted' => string 'Rockets Schedule 2019-2020' (length=26)
      'X-WR-RELCALID' => 
        array (size=1)
          'formatted' => string '77E82E02-8C29-4E94-91B5-DE113AFD9345' (length=36)
      'X-APPLE-CALENDAR-COLOR' => 
        array (size=1)
          'formatted' => string '#B027AE' (length=7)
  'VTIMEZONE' => 
    array (size=3)
      'TZID' => 
        array (size=1)
          'formatted' => string 'America/New_York' (length=16)
      'STANDARD' => 
        array (size=4)
          'DTSTART' => 
            array (size=1)
              'formatted' => int 26532000
          'RRULE' => 
            array (size=1)
              'formatted' => 
                array (size=3)
                  'FREQ' => 
                    array (size=1)
                      'formatted' => string 'YEARLY' (length=6)
                  'BYDAY' => 
                    array (size=1)
                      'formatted' => string '1SU' (length=3)
                  'BYMONTH' => 
                    array (size=1)
                      'formatted' => string '11' (length=2)
          'TZOFFSETFROM' => 
            array (size=1)
              'formatted' => string '-0400' (length=5)
          'TZOFFSETTO' => 
            array (size=1)
              'formatted' => string '-0500' (length=5)
      'DAYLIGHT' => 
        array (size=4)
          'DTSTART' => 
            array (size=1)
              'formatted' => int 5968800
          'RRULE' => 
            array (size=1)
              'formatted' => 
                array (size=3)
                  'FREQ' => 
                    array (size=1)
                      'formatted' => string 'YEARLY' (length=6)
                  'BYDAY' => 
                    array (size=1)
                      'formatted' => string '2SU' (length=3)
                  'BYMONTH' => 
                    array (size=1)
                      'formatted' => string '3' (length=1)
          'TZOFFSETFROM' => 
            array (size=1)
              'formatted' => string '-0500' (length=5)
          'TZOFFSETTO' => 
            array (size=1)
              'formatted' => string '-0400' (length=5)
...
```

The `VEVENT` array looks something like...
```
  'VEVENT' => 
    array (size=93)
      0 => 
        array (size=13)
          'SEQUENCE' => 
            array (size=1)
              'formatted' => int 1
          'DTSTART' => 
            array (size=2)
              'formatted' => int 1562446800
              'params' => 
                array (size=1)
                  'TZID' => 
                    array (size=1)
                      'formatted' => string 'America/New_York' (length=16)
          'STATUS' => 
            array (size=1)
              'formatted' => string 'TENTATIVE' (length=9)
          'DTSTAMP' => 
            array (size=1)
              'formatted' => int 1582285210
          'SUMMARY' => 
            array (size=1)
              'formatted' => string 'Rockets 81, Mavericks 113' (length=25)
          'DTEND' => 
            array (size=2)
              'formatted' => int 1562457600
              'params' => 
                array (size=1)
                  'TZID' => 
                    array (size=1)
                      'formatted' => string 'America/New_York' (length=16)
          'LOCATION' => 
            array (size=1)
              'formatted' => string 'Cox Pavilion, Las Vegas, NV' (length=27)
          'DESCRIPTION' => 
            array (size=1)
              'formatted' => string 'Get tickets for the game: https://a.data.nba.com/tickets/single/2019/1521900017/TEAM_ICS

 Watch the game live on NBATV
www.rockets.com' (length=138)
          'UID' => 
            array (size=1)
              'formatted' => string '20200221T114010185Z-1' (length=21)
          'TRANSP' => 
            array (size=1)
              'formatted' => string 'OPAQUE' (length=6)
          'ORGANIZER' => 
            array (size=1)
              'formatted' => string 'CN=Rockets Schedule 2019-2020' (length=29)
          'CLASS' => 
            array (size=1)
              'formatted' => string 'PUBLIC' (length=6)
          'CREATED' => 
            array (size=1)
              'formatted' => int 1582285210
      1 => 
        array (size=13)
          'SEQUENCE' => 
            array (size=1)
              'formatted' => int 2
          'DTSTART' => 
            array (size=2)
              'formatted' => int 1562536800
              'params' => 
                array (size=1)
                  'TZID' => 
                    array (size=1)
                      'formatted' => string 'America/New_York' (length=16)
          'STATUS' => 
            array (size=1)
              'formatted' => string 'TENTATIVE' (length=9)
          'DTSTAMP' => 
            array (size=1)
              'formatted' => int 1582285210
          'SUMMARY' => 
            array (size=1)
              'formatted' => string 'Rockets 87, Trail Blazers 97' (length=28)
          'DTEND' => 
            array (size=2)
              'formatted' => int 1562547600
              'params' => 
                array (size=1)
                  'TZID' => 
                    array (size=1)
                      'formatted' => string 'America/New_York' (length=16)
          'LOCATION' => 
            array (size=1)
              'formatted' => string 'Cox Pavilion, Las Vegas, NV' (length=27)
          'DESCRIPTION' => 
            array (size=1)
              'formatted' => string 'Get tickets for the game: https://a.data.nba.com/tickets/single/2019/1521900028/TEAM_ICS

 Watch the game live on NBATV
www.rockets.com' (length=138)
          'UID' => 
            array (size=1)
              'formatted' => string '20200221T114010187Z-2' (length=21)
          'TRANSP' => 
            array (size=1)
              'formatted' => string 'OPAQUE' (length=6)
          'ORGANIZER' => 
            array (size=1)
              'formatted' => string 'CN=Rockets Schedule 2019-2020' (length=29)
          'CLASS' => 
            array (size=1)
              'formatted' => string 'PUBLIC' (length=6)
          'CREATED' => 
            array (size=1)
              'formatted' => int 1582285210
...
```

By default, only `formatted` results are returned, which means a few things:

* Numbers are integers
* Date/Times are Unix timestamps
* Strings are stripslashed and trimmed
* Some numbers must exist within certain ranges
* Some strings must be specific values
* Some values consist of multiple other values
