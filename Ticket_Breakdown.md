# Ticket Breakdown
We are a staffing company whose primary purpose is to book Agents at Shifts posted by Facilities on our platform. We're working on a new feature which will generate reports for our client Facilities containing info on how many hours each Agent worked in a given quarter by summing up every Shift they worked. Currently, this is how the process works:

- Data is saved in the database in the Facilities, Agents, and Shifts tables
- A function `getShiftsByFacility` is called with the Facility's id, returning all Shifts worked that quarter, including some metadata about the Agent assigned to each
- A function `generateReport` is then called with the list of Shifts. It converts them into a PDF which can be submitted by the Facility for compliance.

## You've been asked to work on a ticket. It reads:

**Currently, the id of each Agent on the reports we generate is their internal database id. We'd like to add the ability for Facilities to save their own custom ids for each Agent they work with and use that id when generating reports for them.**


Based on the information given, break this ticket down into 2-5 individual tickets to perform. Provide as much detail for each ticket as you can, including acceptance criteria, time/effort estimates, and implementation details. Feel free to make informed guesses about any unknown details - you can't guess "wrong".


You will be graded on the level of detail in each ticket, the clarity of the execution plan within and between tickets, and the intelligibility of your language. You don't need to be a native English speaker, but please proof-read your work.

## Your Breakdown Here

### Ticket 1:
Add a new column `custom_agent_id` to the `Agents` table.

#### Details:
In order to include a custom agent id in the Facilities report, we should first save it in a new database column. Please use this ticket to create a new column in the `Agents` table. Below are the implementation details:

- The column should be a varchar field with max_length = 255 chars.
- The column should be `unique` and `indexed` for faster searching.
- The column should accept only alphabets, numbers and underscore.
- No special characters other than `underscore` are allowed.
- Override the `save()` method of the model to implement the special requirements. 

#### Acceptance criteria:
- A varchar field (255) with the name `custom_agent_id` is created.
- The newly created column has the `unique` constraint and is `indexed`.
- A database migration is created for the new field.
- Only alphabets, numbers and underscore are accepted into the field.
- Special characters when entered prevents the data from being saved into the field.


### Ticket 2:
Add a new text field in the Agent creation form to enter `custom_agent_id`

### Ticket 3:
Display the `custom_agent_id` field and it's value in the generated PDF report.

### Ticket 4:
Update the backend to accept and return the additional field `custom_agent_id` in the JSON payload.

