# CPQ Line Overrides

This is a low-impact solution to manage which users can perform edit or delete actions in the Salesforce UI on Opportunity Lines and Quote Lines (from Salesforce CPQ).

## What Is This?

This is a collection of Visualforce pages and custom permissions for use on the Salesforce Force.com platform. There is no controller or controller extension, by design. This currently has no additional burden to code coverage, making it low-impact and easy to add to any org without creating additional work or possible conflicts.

## What Does This Do?

It provides a way to override edit and delete actions in the Salesforce UI, ensuring users can't circumvent the process and flow of Salesforce CPQ. It also provides a way to allow some users to move forward with custom permissions.

## Why Would You Need This?

When using the Salesforce CPQ package, users should not add, edit, or delete Opportunity Line Items manually anymore. This process should shift to utilizing the CPQ Quote Line Editor screen from a Quote to accomplish the goal of adding, editing, or removing products. You also don't want users to manually edit/delete Quote Line records either, as this is a backdoor that skips some calculation, and allows violation of bundling logic.

You can remove buttons in the Classic or Lightning UI (such as Add Product, Edit All, Edit, Delete, New); but the "Edit | Delete" links will always be present on related list on the Opportunity or Quote.

You could remove these related lists to accomplish your goal, but that takes away some valuable at-a-glance information about the Opportunity/Quote.

What we have done here is create a couple of Visualforce pages that can be used to override the default actions for these objects. This way, anytime a user clicks a link or button that corresponds to the action that is being overriden, they will be presented with the new Visualforce page instead.

We take this chance to provide them with some information about why we are stopping them from taking this action, and provide links to allow them to continue down the correct path, or to go back where they came from. A key here is to not end up with a dead-end for the User; we aren't just telling them no, we are guiding them to the right path.

There are also scenarios where some users may need to be able to take these actions while most users cannot (typically a System Admin, or Sales Operations). To accomodate this, we have included two Custom Permissions (one for Edit and Delete respectively). These Custom Permissions can be assigned via Permission Set or directly on a Profile. Users with these permissions will still see the Visualforce page, but will be presented a third button to force their way through to the appropriate action. This way not functionality is lost for the users that do need this ability, while controlling the rest of the users.

## How Can This Be Improved?

* Use of Custom Labels for text to be compatible with multi-language
* Clean up navigation methods to be consistent
* Revise text
* Controlling showheader and showsidebar dynamically based on the UI (so these aren't shown in LEX)
* Possibly having the page navigate to the original target immediately for users with the correct permission (using the 'action' attribute on the page)
* Probably could use some CSS clean-up
* First Github repo ever, so this readme and other things at the repo level could probably be improved
* I am not aware of a way to check the SBQQ version number without adding a controller. If there is a way to get that in the page, could make it more robust with the major UI change from version 26 to 27 (the VF page used for the QLE changes between these versions)

## Getting Started

### Prerequisites

This requires the Salesforce CPQ package, and Salesforce Professional Edition or higher. Spring '17 release for Salesforce is required for the Lightning Design System used in these pages.

### Installing

You can either add these items in the GUI, or deploy with an IDE.

To use the IDE, add these items to your project and package.xml, and push to your org.

To add them manually in the GUI, first recreate the Custom Permissions. Follow the info here, including the API name. This repo has the Delete permission dependent on the Edit permission. You can adjust that as needed.

After both Custom Permissions are created, then create two Visualforce pages, pasting in the code from here for each. Name them to match the file names here, or use your own naming if desired.

If you used a different name for the Custom Permissions, you will need to edit the Visualforce content to reference your permissions instead.

The code provided here works for Salesforce CPQ version 27+. If you are using an older version, you will need to replace 'SBQQ__sb' with 'SBQQ__EditQuoteLines' in the Visualforce code; as the Quote Line Editor page is different.

The code provided here is focused on Opportunities and Quotes from Opportunities. If you have a different configuration, you will need to customize the code here to fit your objects.

After the Permissions and Pages have been created, edit the Security settings for the Visualforce pages, and ensure all profiles have access to the pages. You can share with only a few profiles, but it's safer to share with all.
Then add the Custom Permission to either Permission Sets or Profiles. Users with these Permissions Sets or Profiles will have access to continue past the Visualforce page.

To set the action override, first go to the Opportunity 'Buttons, Links, and Actions' area (how you get there depends on your UI, but in general search for Opportunity in setup, and find this section). Next to the 'Edit' and 'Delete' buttons, click 'Edit'. Here you can choose how this action is handled. We want to choose a Visualforce page, and select the page created here. Then save. Repeat for both actions.
Then repeat this process for the Quote object (this is a custom object, from the Salesforce CPQ package, not the standard Quote object).

Now anyone trying to edit or delete these records directly, outside of the Quote Line Editor will see the Visualforce pages instead of being able to make changes.

## Contributing

Please read [CONTRIBUTING.md](CONTRIBUTING.md) for details on our code of conduct, and the process for submitting pull requests to us.


## Authors

* **Michael Mock** - *Initial work* - [Mock1](https://github.com/Mock1)

See also the list of [contributors](https://github.com/Mock1/CPQLineOverrides/contributors) who participated in this project.

## License

This project is licensed under the GNU License - see the [LICENSE](LICENSE) file for details

## Acknowledgments

* Original idea came from Stacy Hyatt @ Birlasoft - she's pretty awesome
