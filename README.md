This project is a web interface that lets you do two things:
- Turn the broken URLs on [UIUC Course Information Suite(CIS) API documentation](https://courses.illinois.edu/cisdocs/api) into working `cisapp/explorer` URLs
- Turn working `cisapp/explorer` course URLs into URLs for `courses.illinois.edu/search` pages

It does this with a detailed error output whenever possible.

This app effectively functions as interface for the [cisurls module](https://www.npmjs.com/package/cisurls). The module used to be part of this app but I turned it into a module to allow me to use the functions in other projects.

This project also holds the `cisapi.xsd` schema file([mirth.cc/cisurls/cisapi.xsd](https://mirth.cc/fix-cisapi/cisapi.xsd)), which is a broken link in the documentation and cannot be found elsewhere as of 2023-03-22. 

The documentation fix tool does the following:
- Change “/cisapi” to “/cisapp/explorer”
- Add `.xml` before the parameters. For example, change “/courses” to “/courses.xml”
- And it can also enable cascade mode on any URL you give it

> ⚠️ NOTE: There may exist URLs in the documentation that have errors besides these. This tool does not and will not fix them. This tool *does*, however, fix the error on the schedule/courses page(missing ampersands before section symbols)
