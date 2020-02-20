# iCalendar to PHP

Parse an iCalendar URI, with PHP, into some useful format.

In a WordPress plugin, you can test this quickly with something like...

```
add_action( 'init', function() {

	// Pick a URI
	$uri = 'https://data.nba.net/prod/teams/schedules/2019/rockets_schedule.ics';

	// Load it
	$ical = new Sugar_Calendar\Utilities\iCalendar\ToArray( $uri );

	// Sort all events
    $ical_data = $ical->get_list_sort();

	// Dump it out
	var_dump( $ical_data ); die;
} );
```
