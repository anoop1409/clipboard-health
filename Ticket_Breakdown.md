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

- The column should be a `varchar` field with max_length = 255 chars.
- The column should be `unique` and `indexed` for faster searching.
- It should be a `nullable` field as there are existing rows and the field is optional.
- The column should accept only alphabets, numbers and underscore.
- No special characters other than `underscore` are allowed.
- Override the `save()` method of the model to implement the special requirements. 
- Update the existing tests and add new tests to incorporate the change.

#### Acceptance criteria:
- A varchar field (255) with the name `custom_agent_id` is created.
- The newly created column has the `unique` constraint and is `indexed`.
- A database migration is created for the new field.
- Only alphabets, numbers and underscore are accepted into the field.
- Special characters when entered prevents the data from being saved into the field.
- New tests are added to cover the impact of the newly implemented column.

#### Estimate:
1 day - This includes adding new tests and accomodating the existing tests to match the changes.


### Ticket 2:
Add a new text field in the Agent creation and edit form to enter or update the `custom_agent_id` value.

#### Details:
Create a new field `Custom agent id` that takes the input from the user while creating a new agent or while editing an existing agent.


- When the user is on the edit form, pre-populate the value from the database.
- Validate the input to allow only alphabets, numbers and underscore.
- Any other special characters when entered, should display an error message: `"Invalid agent id. Only alphabets, numbers and underscore are allowed"`
- Update the request payload to include the `custom_agent_id` field and its value.
- Update the tests and add new tests for the newly introduced field.

#### Acceptance criteria:
- Agent creation form displays the `Custom agent id` field.
- The field accepts only alphabets, numbers and underscores.
- When an invalid text is entered, it displays the error message as expected.
- On clicking the `Save` button, value gets saved into the table.
- Agent edit form displays the `Custom agent id` field with pre-populated value.
- The field accepts only alphabets, numbers and underscores.
- When an invalid text is entered, it displays the error message as expected.
- On clicking the `Save` button, value gets saved into the table.

#### Estimate:
1 day - This includes adding new tests and accomodating the existing tests to match the changes. Also, considering that the form for the agent creation and edit are using the same components and hence the changes in one form is reflected in the other automatically.


### Ticket 3:
Update the backend to accept and return the additional field `custom_agent_id` in the JSON payload.
#### Details:
The frontend will send the value for the newly introduced field - `custom_agent_id` going forwards. The backend system should be updated accordingly to accept the newly introduced field in the request payload and also to provide the value of this field in the response payload where required.

- Accept the `custom_agent_id` property in both the `POST` and `PUT` endpoints for the `Agent` model.
- Update the `generateReport` method to return the `custom_agent_id` as part of the response.
- Update the Serializer to mark this change.
- Update the tests
- Add new tests to cover the change

#### Acceptance criteria:
- `custom_agent_id` is accepted and saved by both the `POST` and `PUT` endpoints of the `Agent` model.
- `generateReport` method returns the `custom_agent_id` along with the rest of the data.
- The API endpoint - `/report` returns the `custom_agent_id` field and the corresponding value from the database.

#### Estimate:
0.5 day - This includes adding new tests and accomodating the existing tests to match the changes


### Ticket 4:
Display the `custom_agent_id` field's value in the generated PDF report when available.

#### Details:
The newly introduced `custom_agent_id` should replace the currently used `agent_id` in the Facilities report if exists. 

- If the value of `custom_agent_id` is `null` then continue to display the `agent_id`.
- If the value of `custom_agent_id` is not `null` then display the `custom_agent_id` instead of `agent_id`.
- Update the tests and add new tests.

#### Acceptance criteria:
- The report displays the `custom_agent_id` value when present.
- The report displays the `agent_id` value when `custom_agent_id` is not present.


#### Estimate:
0.5 day - This includes adding new tests and accomodating the existing tests to match the changes. Also, some functional testing to ensure that the layout of the generated pdf is as expected.

