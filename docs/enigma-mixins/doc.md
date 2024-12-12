# Doc

Global mixin are available on the `doc` enigma.js object.

```js
import { docMixin } from "enigma-mixin";

const session = enigma.create({
  schema,
  mixins: [...docMixin],
  url: `wss://sense-engine-url`,
  createSocket: (url) => new WebSocket(url),
});

const global = await session.open();
const doc = await global.openDoc(); // mixin available once the document is open
```

## Mixin

Logically the mixins are separated in the following categories:

- Selections
- TableAndFields
- Variables
- Extensions
- Bookmarks
- Build/Unbuild

### Selections

#### mSelectionsAll

Returns the current selections (if any). This method will return all selected values (even if more than 6 values are selected)

#### mSelectionsFields

Array of the fields, having selections in them

#### mSelectionsSimple

Returns array of `field <-> selected value`

- `groupByField` (`optional`. default is `false`) If this argument is provided then the array will be grouped by field name:

  ```javascript
  [
    {field: "Field 1": values: [...]},
    {field: "Field 2": values: [...]},
    ...
  ]
  ```

#### mSelectInField

Select a value(s) in the specified field

- field - field name
- values - array of string values to be selected
- toggle (`optional`. default `false`)

### TableAndFields

#### mGetTablesAndFields

Object array with the field <-> table relation

#### mGetTables

Array with all table names

#### mGetFields

Array with all field names

#### mGetSyntheticTables

Array of table names for which `qIsSynthetic == true`

#### mCreateSessionListbox

Creates a **session** listbox object for the specified field

- fieldName
- options (`optional`)

  - type - (`optional`. default `session-listbox`) - type of the listbox
  - state - (`optional`. default `$`) - in which state to create the listbox
  - destroyOnComplete - (`optional`) - if set to `true` once all the operations are complete the session object will "self-destruct". All the info will be returned as normal
  - getAllData - (`optional`) - if set to `true` all values will be extracted. Qlik can extract max 10 000 cells of data on a single call. If the data is more than 10 000 data cells then paging have to be implemented. This method by default will make the initial request with 10 000 cells as initial data fetch and if this argument is passed and set to `true` then the method will request and return all the data from Qlik

> **Note**
> Additionally to the `obj`, `prop` and `layout` this method will expose an extra function (`flattenData`) that can be used to flatten all the existing data. By default all data will be inside the returned `layout` property in its original format. Once `flattenData` function is called it will return all the data in array of `qMatrix` objects (easier to read/use in this format). To avoid duplication and increased resource usage (especially for fields with high number of distinct values) its up to the developer to decide when/if to call this function.

### Variables

#### mVariableGetAll

List with all variables in the document

- showSession (`optional`. default `false`)
- showConfig (`optional`. default `false`)
- showReserved (`optional`. default `false`)

#### mVariableUpdateById

Update existing variable by id

- id - id of the variable, to be updated
- definition - the expression/definition of the variable
- comment - (`optional`)

#### mVariableUpdateByName

Update existing variable by name

- name - name of the variable, to be updated
- definition - the expression/definition of the variable
- comment - (`optional`)

#### mVariableCreate

Create new variable

- name - (`optional`. default is an empty string)
- comment (`optional`. default is an empty string)
- definition (`optional`. default is an empty string)

### Extensions

!!! warning "EXPERIMENTAL"

    Do not over-trust the returned data

#### mExtensionObjectsAll

List with all extensions objects in the document

### Bookmarks

!!! warning "EXPERIMENTAL"

    Do not over-trust the returned data

#### mGetBookmarksMeta

Returns full info about all bookmarks (to which the user have access)

- `state` (`optional`. default is `$`)
  Each row will return:
  - `layout` - layout of the Qlik object (`getLayout()`)
  - `properties` - properties of the Qlik object (`getProperties()`)
  - `setAnalysis` - raw set analysis returned from Qlik. For example: `"<state_name={'California','Minnesota','Ohio','Texas'}>"`
  - `setAnalysisDestructed` - set analysis but in more readable format. Using the example above:
    - field: `state_name`
    - type: `field`
    - values: `California`,`Minnesota`,`Ohio`,`Texas`

#### mGetBookmarkMeta

Similar to `mGetBookmarksMeta` but returns the meta for only one bookmark

- `bookmarkId`
- `state` (`optional`. default is `$`)

#### mCreateBookmarkFromMeta

Create new bookmark using data from an existing bookmark

- `bookmarkMeta` -
- `title` -
- `description` (`optional`. default is empty string)

#### mGetBookmarkValues

Return the values for a specific bookmark

- `bookmarkId`
- `state` (`optional`. default is `$`)

#### mCloneBookmark

Create new bookmark using data from existing bookmark (kinda similar to

#### mCreateBookmarkFromMeta

In the near future only one of these methods will stay)

- `sourceBookmarkId` -
- `state` -
- `title`
- `description` (`optional`. default is empty string)

### Build/Unbuild

!!! warning "EXPERIMENTAL"

    Do not over-trust these methods and always make a copy of the app(s)

If you haven't used [qlik-cli](https://qlik.dev/toolkits/qlik-cli/) now its a good time to start :) But I've wanted to have the same/similar functionality (for building/unbuilding app) in [enigma.js](https://github.com/qlik-oss/enigma.js/blob/master/schemas/12.67.2.json) as well.

#### mUnbuild

Extracts all parts of a Qlik Sense app (apart from the data itself) into JSON object. The resulted JSON object have the following format:

- appProperties (`object`)
- connections (`array`)
- dimensions (`array`)
- measures (`array`)
- objects (`array`) - all other objects. Including sheets, charts, filters etc.
- script (`string`)
- variables (`array`)

The result JSON can be stored in file (or multiple files) and put under version control.

#### mBuild

Opposite to `unbuild`. Provide a JSON object with Qlik objects and this mixin will update the existing objects or, if they do not exists, it will create them. The structure of the input JSON object must be in specific format (see `unbuild`).

!!! note

    To preserve the changes the app have to be saved. The build command do not save the app automatically!
