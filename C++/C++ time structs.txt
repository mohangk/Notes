time_t
	it is generally implemented as an integral value representing the number of seconds elapsed since 00:00 hours, Jan 1, 1970 UTC
 
struct tm 
	Structure containing a calendar date and time broken down into its components.


timeval
	time_t tv_sec
	long int tv_usec

timespec
	time_t tv_sec
	long int tv_nsec


mktime
	Convert tm structure to time_t (function )
localtime
	Convert time_t to tm as local time (function )
gmtime
	Convert time_t to tm as UTC time (function )
