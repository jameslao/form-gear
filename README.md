# Form Gear

# About



FormGear is a framework engine for dynamic form creation and complex form processing and validation for data collection. It is easy to use and efficiently handle nested inquiries to capture everything down to the last detail. Unlike other similar framework, validation is handled in a FALSE condition in which each field is validated against a test equation. This leads to a more efficient and effective way to validate each component.



# Usage



## Installation

FormGear can be installed via package manager like `npm` or `yarn`, or it can be used directly via CDN.

```bash
npm install form-gear
```

```jsx
import { FormGear } from 'form-gear'
import {  } from './node_modules/form-gear/dist/style.css'

const data = Promise.all([
                fetch("./data/template.json").then((res) => res.json()),
                fetch("./data/preset.json").then((res) => res.json()),
                fetch("./data/response.json").then((res) => res.json()),
                fetch("./data/validation.json").then((res) => res.json())
                fetch("./data/remark.json").then((res) => res.json())
            ]);

data.then(([template, preset, response, validation, remark]) => initForm(template, preset, response, validation, remark));

function initForm(template, preset, response, validation, remark){
        
   let config = {
      clientMode: 1, // CAWI = 1, CAPI = 2
      token: `aeyJ0eXA`,
      baseUrl: `https://api-survey.bps.go.id/designer/api/lookup-data/json`,
      lookupKey: `key%5B%5D`,
      lookupValue: `value%5B%5D`,
      username: 'AdityaSetyadi'
   }
   
   let uploadHandler = function (setter) {
      console.log('camera handler', setter);
      cameraFunction = setter;
      openCamera();
   }

   let GpsHandler = function (setter, isPhoto) {
      console.log('camera handler', setter);
      isPhoto = true,
         cameraGPSFunction = setter;
      openCameraGPS(isPhoto);
   }

   let onlineSearch = async (url) =>
      (await fetch(url, setBearer())
         .catch((error: any) => {
            return {
               success: false,
               data: {},
               message: '500'
            }
         }).then((res: any) => {
         if (res.status === 200) {
            let temp = res.json();
            return temp;
         } else {
            return {
               success: false,
               data: {},
               message: res.status
            }
         }
         }).then((res: any) => {
            return res;
         }
      ));

   let setResponseMobile = function (res, rem) {
      respons = res
      remarks = rem

      console.log('respons', respons)
      console.log('remarks', remarks)
   }

   let setSubmitMobile = function (res, rem) {
      respons = res
      remarks = rem

      console.log('respons submit', respons)
      console.log('remarks submit', remarks)
   }

   let openMap = function (koordinat) {
      koordinat = koordinat

      console.log('coordinat ', koordinat)
   }

   let form = FormGear(template, preset, response, validation, remark, config, uploadHandler, GpsHandler, onlineSearch, setResponseMobile, setSubmitMobile, openMap);
   
   return form;
}

```

# Template



FormGear use defined template which is based on JSON Object. 

```
template.json
│   description
│   dataKey
│		title
│		acronym
│
└───components
│   │   section
│   └───components
│       │   textInput
│       │   checkboxInput
│       │   selectInput
│       │   ...
└───
│   │   section
│   └───components
│       │   textInput
│       │   checkboxInput
│       │   nestedInput
│       └───
│           │   sourceQuestion
│           └───components
│               │   textInput
│               │   checkboxInput
│               │   nestedInput
│               └───
│                   │   sourceQuestion
│                   └───components
│                       │   textInput
│                       │   dateInput
│                       │   ...
│               │   ...
│       │
│       │   dateInput
│       │   ...
└───
│   │   section
│   └───components
│       │   textInput
│       │   checkboxInput
│       │   selectInput
│       │   ...
│

```

### Control Type

---

| Control Type | Code | Description |
| --- | --- | --- |
| Section | 1 | Adds section to form. |
| NestedInput | 2 | Adds nested input field to existing field. |
| InnerHTML | 3 | Adds text written in HTML format. |
| VariableInput | 4 | Variable input. |
| DateInput | 11 | Date input. |
| DateTimeLocalInput | 12 | Date and time input field with no time zone. |
| TimeInput | 13 | Time input field. |
| MonthInput | 14 | Month input field. |
| WeekInput | 15 | Week input field. |
| SingleCheckInput | 16 | Checkbox input field, lets user choose only one option out of limited choices. |
| ToggleInput | 17 | Toggle input. |
| RangeSliderInput | 18 | Range slider input, lets user to select a value or range of value from a specified min and max. |
| UrlInput | 19 | URL input field. |
| CurrencyInput | 20 | Currency input field, limited to IDR and USD. |
| ListTextInputRepeat | 21 | Text input field, creating a list by letting user add more written input as needed. |
| ListSelectInputRepeat | 22 | Dropdown input field, creating a list by letting user add more choices as needed. |
| MultipleSelectInput | 23 | Drop-down input, lets user to choose one or more options of limited choices. |
| MaskingInput | 24 | Number input with certain formatting, such as phone number and taxpayer identification number. |
| TextInput | 25 | Single-line input field. |
| RadioInput | 26 | Radio button, lets user choose one options of limited choices. |
| SelectInput | 27 | Drop-down input. |
| NumberInput | 28 | Numeric input field. |
| CheckboxInput | 29 | Checkbox input field, lets user choose one or more options of limited choices. |
| TextAreaInput | 30 | Adjustable text area input field. |
| EmailInput | 31 | Email address input field. |
| PhotoInput | 32 | Photo input, lets user add picture with .jpg, .jpeg, .png, and .gif format. |
| GpsInput | 33 | GPS input. |

### Option

---

Define option properties for `ListSelectInputRepeat`, `MultipleSelectInput`, `RadioInput`, and `CheckboxInput` input.

- `label`: label of an option that will show up in the form.
- `value`: value of an option that will be recorded.
- `open`: define whether or not an option is open-ended. The value recorded will be

### Range

---

Define properties for `RangeSliderInput` input.  

- `min`: minimum of a range.
- `max`: maximum of a range.
- `step`: added value for each step.

### Select Option

---

Define properties for `sourceSelect` input.

- `id`: identifier of the lookup table that will be used.
- `tableName`: the name of the lookup table that will be used.
- `value`: the column name in the lookup table that will be recorded as the option’s value.
- `desc`: the column name in the lookup table that will be shown as the option’s label.
- `parentCondition`: an array consisting of `key` and `value`. `key` refers to the parent lookup table’s `value`, while `value` refers to the parent lookup table’s `desc`.

### Component Type

---

| Component Type | Corresponding ControlType  | Input Type | Description | Notes |
| --- | --- | --- | --- | --- |
| dataKey | All | string | Component identifier. |  |
| label | All | string | Component label that will show up in the form. |  |
| hint | All | string | Provide hint on how to fill the corresponding field. |  |
| type | All | ControlType | any | Define the ControlType of a field. |  |
| components | 1, 2 | ComponentType | Store components inside a Section or NestedInput. |  |
| rows | 30 | number | Define the number of rows needed in TextAreaInput. |  |
| cols | 26, 29 | number | Define the number of columns the options in RadioInput and CheckboxInput will be divided to. |  |
| options | 22, 23, 26, 29 | Option[] | Define options for ListSelectInputRepeat, MultipleSelectInput, RadioInput, and CheckboxInput.  |  |
| range | 18 | Range[] | Define the min, max, and value of each step for a RangeSliderInput. |  |
| description | 1, 2 | string | Adds description for a Section or NestedInput. |  |
| answer | All | any | Storing answer collected on data collection. | ListSelectInputRepeat  and MultipleSelectInput must add: [{"label": "lastId#0","value": "0"}] |
| sourceQuestion | 2 | string | Define the source question for a NestedInput field. |  |
| sourceOption | 22, 23, 26, 27, 29 | string | Source of the options used in the field. |  |
| typeOption | 22, 23, 26, 27, 29 | number | Type of the options used in the field. | 1: Its component.
2: A lookup table.
3: Other component. |
| currency | 20 | string | Define the currency that will be used in a CurrencyInput field, limited to IDR and USD. |  |
| separatorFormat | 20 | string | Define the separator that will be used in a CurrencyInput field. |  |
| isDecimal | 20 | boolean | Define whether or not a CurrencyInput field allow decimal. |  |
| maskingFormat | 24 | string | Define the type of maskingInput used. |  |
| expression | 4 | string | A variable expression. | getValue function can be used to get the value of other field. |
| componentVar | 4 | string[] | Define the component(s) used in VariableInput. |  |
| render | 4 | boolean | Define whether or not the variable will be rendered. |  |
| renderType | 4 | number | Variable output type. | 1: single output
2: array |
| enable | All | boolean | Define whether or not a component have a condition(s) that need satisfied before it can be filled. |  |
| enableCondition | All | string | An expression to define a condition enabled for the field to be filled. | getProp function can be used to get the client mode.
getValue function can be used to get the value of other field. |
| componentEnable | All | string[] | A list of component(s) used in a condition expression. |  |
| enableRemark | All | boolean | Define whether or not a component allows a remark. |  |
| titleModalDelete | 21, 22 | string | Title of the warning that will show up when user tries to delete an item in ListTextInputRepeat or ListSelectInputRepeat. |  |
| contentModalDelete | 21, 22 | string | Content of the warning that will show up when user tries to delete an item in ListTextInputRepeat or ListSelectInputRepeat. |  |

## Preset



Preset is used to provide prefilled data given prior to data collection.  This prefilled data usually obtained from previous data collection or a listing conducted before the actual data collection. Preset consists of`dataKey` of the corresponding field and the prefilled`answer` to that field.

```
preset.json
│
└───predata
│   │
│   └───
│       │   dataKey
│       │   answer
│   │
│   └───
│       │   dataKey
│       │   answer
│
```

```json
{
   "description":"sample template",
   "dataKey":"sample_tpl1",
   "predata":[
      {
         "dataKey":"nama_lengkap",
         "answer":"Setyadi"
      }
   ]
}
```

# Response



Response is used to store any response given during data collection. Response consists of`dataKey` of the corresponding field and the `answer` collected from that field. `answer` from a field that has both `label` and `value` will record both. `answer` will only be collected if the value entered to the corresponding field satisfied both the condition enabled and validation test function.

```
response.json
│
│		description
│   dataKey
│   templateVersion
│   validationVersion
│   createdBy
│   editedBy
│   createdAt
│   lastUpdated
└───answers
│   │
│   └───
│       │   dataKey
│       │   answer
│   │
│   └───
│       │   dataKey
│       │   answer
│

```

```json
{
   "description":"sample template",
   "dataKey":"sample_tpl1",
   "answers":[
      {
         "dataKey":"agree",
         "answer":true
      },
      {
         "dataKey":"nm_lengkap",
         "answer":"Agung"
      },
      {
         "dataKey":"anggota",
         "answer": {
            "value" : "3",
            "label" : "Clementine Bauch"
         }
      },
      {
         "dataKey":"alamat",
         "answer":"Jalan Otista"
      },
      {
         "dataKey":"penggunaan_lahan",
         "answer":[
            {
               "label": "lastId#1",
               "value": "0"
            },
            {
               "label": "Sawah",
               "value": "1"
            },
            {
               "label": "Padang Rumput Permanen",
               "value": "3"
            }
         ]
      },
   ]
}

```

# Validation



Validation is used to validate the answer given during data collection against a test function. The validation is handled in a FALSE condition, meaning the test function is a condition where the answer of a question field would be false. 

- `dataKey`: component identifier.
- `validations`: store validation for each `dataKey`.
- `componentValidation`: a list of the `dataKey` of components used in the test function.
- `test`: the test function. A `getValue` function can be used to get the value of other field.
- `message`: the warning message that will show up when the value entered did not fulfill the test function.
- `type`: define whether the validation will be just a warning or an error.

```
validation.json
│
└───description
│		dataKey
│		version
│		testFunctions
│   │
│   └───
│       │   dataKey
│       │   componentValidation
│       │   validations
│				   │
│				   └───
│				       │   test
│				       │   message
│				       │   type
│				   │
│				   └───
│				       │   test
│				       │   message
│				       │   type
│
```

```json
{
    "description":"sample template",
    "dataKey":"sample_tpl1",
    "version":"0.0.1",
    "testFunctions":[
        {
            "dataKey":"usia",
            "componentValidation":["usia"],
            "validations": [
                {
                    "test":"getValue('usia') >= 8 && getValue('usia') < 10",
                    "message":"Usia minimal 10 tahun Usia minimal 10 tahun Usia minimal 10 tahun Usia minimal 10 tahun Usia minimal 10 tahun Usia minimal 10 tahun ",
                    "type":1
                },
                {
                    "test":"getValue('usia') < 8",
                    "message":"Usia minimal 10 tahun",
                    "type":2
                }
            ]
        }
		]
}
```

# Remark



```
remark.json
│
└───dataKey
|		notes
│   │
│   └───
│       │   dataKey
│       │   comments
│				   │
│				   └───
│				       │   sender
│				       │   dateTime
│				       │   comment
│				   │
│				   └───
│				       │   sender
│				       │   dateTime
│				       │   comment
│
```

```json
{
    "dataKey": "",
    "notes": [
        {
            "dataKey": "nama_lengkap",
            "comments": [
                {
                    "sender": "AdityaSetyadi",
                    "datetime": "2022-03-17 15:09:54",
                    "comment": "Based on the ID Card"
                },
                {
                    "sender": "AdityaSetyadi",
                    "datetime": "2022-03-18 10:10:40",
                    "comment": "Not the same with his driving license"
                }
            ]
        },
        {
            "dataKey": "luas_lahan#1",
            "comments": [
                {
                    "sender": "AdityaSetyadi",
                    "datetime": "2022-03-18 01:09:15",
                    "comment": "Unknown"
                }
            ]
        }
    ]
}
```

# License



# Our Team

