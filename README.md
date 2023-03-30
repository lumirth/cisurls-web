# CISURLS Web Interface
`cisurls-web` is an interface that lets you do two things:
- Turn the broken URLs on [UIUC Course Information Suite(CIS) API documentation](https://courses.illinois.edu/cisdocs/api) into working `cisapp/explorer` URLs
- Turn working `cisapp/explorer` course URLs into URLs for `courses.illinois.edu/search` pages

It does this with a detailed error output whenever possible.

It effectively functions as an interface for the [cisurls module](https://www.npmjs.com/package/cisurls). The module's functionality used to be part of this app, but it was separated to allow for reuse.

Also, the `cisapi.xsd` schema file([mirth.cc/cisurls/cisapi.xsd](https://mirth.cc/fix-cisapi/cisapi.xsd)) can be found on this website. It is a broken link in the documentation and cannot be found elsewhere as of 2023-03-22. 

---

The documentation fix tool does the following:
- Change “/cisapi” to “/cisapp/explorer”
- Add `.xml` before the parameters. For example, change “/courses” to “/courses.xml”
- And it can also enable cascade mode on any URL you give it

> ⚠️ **NOTE**: There may exist URLs in the documentation that have errors besides these. This tool does not and will not fix them. This tool *does*, however, fix the error on the schedule/courses page(missing ampersands before section symbols)


# Example URLs

Here are some test URLs. Try them at [mirth.cc/cisurls-web](https://mirth.cc/cisurls-web)

For documentation URL fixes:

- http://courses.illinois.edu/cisapi/schedule?mode=summary
- http://courses.illinois.edu/cisapi/schedule/2012?mode=links
- http://courses.illinois.edu/cisapi/schedule/2012/spring?mode=summary
- http://courses.illinois.edu/cisapi/schedule/2012/spring/AAS?mode=summary
- http://courses.illinois.edu/cisapi/schedule/2012/spring/AAS/100?mode=summary
- http://courses.illinois.edu/cisapi/schedule/2012/spring/AAS/100/30107
- https://courses.illinois.edu/cisapi/schedule/courses?year=2012&term=spring§ionTypeCode=LEC§ionTypeCode=Q&collegeCode=KV&creditHours=3&subject=CHEM&sessionId=1&gened=NAT&qp=atomic+structure
- http://courses.illinois.edu/cisapi/schedule/sections?year=2012&term=spring&crn=48596&crn=38329&crn=38345&crn=39192&crn=32484&crn=32491&crn=53063&crn=32787
- http://courses.illinois.edu/cisapi/schedule/sessions/2012/spring

For course URL conversion:
- https://courses.illinois.edu/cisapp/explorer/schedule/2012/spring/AAS/100.xml
- https://courses.illinois.edu/cisapp/explorer/schedule/2023/spring/CS/222.xml?mode=summary
- https://courses.illinois.edu/cisapp/explorer/schedule/2023/spring/cs/361.xml
- https://courses.illinois.edu/cisapp/explorer/schedule/2012/spring/AAS/100.xml?mode=cascade
- https://courses.illinois.edu/cisAPP/expLORER/SChedULE/2012/SPRING/aas/100.xml?mode=cascade

# Notes

- Fixing documentation URLs might be less forgiving about case than course URL conversions because course URL conversions use a more consistent format
