[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](http://makeapullrequest.com)

# FabLab Booking Bot

## Build and run

### Requirements

1. Docker 1.6.2 or above.
1. Google Authenticate file. Check [this instruction](https://github.com/FabLab61/bookingbot/wiki/Google-Authenticate-file) to create it.

### Steps

1. Clone the repository.
1. Put Google Authenticate file into repo dir. Rename it to `gapi.conf`.
1. Replace Telegram token with yours in configuration file (see `Configuration` section below).
1. Run `dockerctl.sh` (you have to be `root` or member of `docker` group). It will take some time to download Docker images and Perl modules.

_Run `dockerctl.sh help` for more run options._

## Configuration

Configuration file named `bot.pl.json` and has following structure (comments separated with `#`):

```
{
	"token": "<...>",                      # Telegram Bot API token.

	"language": "Russian",                 # Optional. Bot default language
	                                       # Supported values: "English" (default), "Russian".
	"timezone": "Europe/Moscow",           # Bot time zone.
	"workinghours": "08:00-00:00",         # Working hours of your organization
	                                       # in format HH:MM-HH-MM (24 hours).

	"resources": {
		"CTC 3D printer": {                # Human-readable name of resource.
			"calendar": "primary"          # Resource's calendar id ("primary"
	                                       # for primary account calendar).
		},
	},

	"durations": {                         # Available booking durations in form
		"30_min": 30,                      # LANGUAGE_ID => MINUTES.
		"1_hour": 60,
		"2_hours": 120,
		"3_hours": 180
	},

	"instructors": {                       # List of registered instructors.
		"email@example.com": {             # Human-readable instructor id (username, email, etc.).
			"id": "123456789",             # Instructor's Telegram user id.
			"phone_number": "12345678901", # Instructor's phone number
			"first_name": "Neil",          # and name.
			"last_name": "Gershenfeld"
		}
	}
}
```

## Usage

1. Register your instructors in `bot.pl.json` and run the bot.

1. The bot will notify registered instructors when someone book resource in their schedule, so instructors have to open a chat with bot and send `/start` command to it (Telegram bots can't write to humans first). Instructors can create or remove records in their schedules using the bot (every record will be synced with Google Calendar of the resource).

1. You can add the bot to any Telegram group and it will post information about every book he received. This could be useful for common instructors group.

## Software architecture

Bot creates a finite state machine when user send first message

Based on user role InstructorFSM or UserFSM object instance was created. 

Decision made by FSMFactory::create() functon.

Both finite state machines are objects of FSA::Rules class.

FSA::Rules uses named states so that it's easy to tell what state you're in and what state you want to go to

Also BaseFSM provide common methods for InstructorFSM or UserFSM

When creating InstructorFSM and UserFSM objects are accepting as parameter name of class, methods from what will be used in do() and rules() subroutines of FSA::Rules




## Contacts

[Join us](https://t.me/joinchat/AAAAAAkdgZL7if9p7nvqGw) in Telegram (use English or Russian language, please). Questions, feature and pull requests are welcome!
