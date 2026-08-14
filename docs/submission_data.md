# Submission Data (Version 0.0.3)

This document defines the data structure of submission data for `data_version` `0.0.3`.

The `data` object contents are determined by the `type` in the submission payload:

- [`uk.gov.ons.edc.eq:surveyresponse` data](#survey-response-data)
- [`uk.gov.ons.edc.eq:feedback` data](#feedback-data)

## Survey Response Data

**Note:** The data structure used is designed and optimised for the purposes of generic functionality within EQ. It is not the responsibility of EQ to carry out any transforms on submitted response data beyond its native data models, nor on any claims provided by the launching service and included in the response data. Any transforms will need to be carried out by downstream systems as required.

For a submission payload `type` of `uk.gov.ons.edc.eq:surveyresponse` this will contain a `lists` array and an `answers` array.

- `lists`: An array of list objects built up during questionnaire completion
  - `name`: the name of the list (e.g. `people-who-live-here`)
  - `items`: an array of strings of the item identifiers in the list
  - `primary_person`: [optional] the item identifier of the primary person in the list
- `answers`: An array of answer objects containing the answers provided during questionnaire completion
  - `answer_id`: the answer identifier (e.g. `job-description-answer`)
  - `value`: the value of the answer(s) provided for the `answer_id`
  - `list_item_id`: [optional] the ID of the list item the answer was provided for (if answering in the context of a list item)

Structured like this:

```json
"data": {
    "lists": [
        ...
    ],
    "answers": [
        ...
    ],
}
```

### Lists Array Example

```json
"lists": [
    {
        "name": "household",
        "primary_person": "AUZvFL",
        "items": [
            "AUZvFL",
            "yuRiRs"
        ]
    },
    {
        "name": "visitor",
        "items": [
            "vgeYGW"
        ]
    }
]
```

### Answers Array Example

```json
"answers": [
    {
        // Example of a free text input box question
        "value": "piloting space shuttles",
        "answer_id": "job-description-answer"
    },
    {
        // Example of a single value for a radio button question
        "answer_id": "marriage-type-answer",
        "value": "Married"
    },
    {
        // Example of multiple values for a checkbox question
        "value": ["Eggs", "Bacon", "Spam"],
        "answer_id": "favourite-breakfast-food"
    },
    {
        "answer_id": "first-name",
        "value": "Colin",
        "list_item_id": "AUZvFL"
    },
    {
        "answer_id": "last-name",
        "value": "Cat",
        "list_item_id": "AUZvFL"
    },
    {
        "answer_id": "first-name",
        "value": "Dave",
        "list_item_id": "yuRiRs"
    },
    {
        "answer_id": "last-name",
        "value": "Dog",
        "list_item_id": "yuRiRs"
    }
]
```

### Answers Array Example (list item based relationship type)

```json
"answers": [
    {
        // example of the list based relationship answser value array
        // based on a mother, father and 2 children
        "answer_id": "relationship-answer",
        "value": [
            {
                // Father's relationship to mother
                "list_item_id": "tkziBG",
                "to_list_item_id": "jBlqGM",
                "relationship": "Husband or Wife"
            },
            {
                // Father's relationships to child 1
                "list_item_id": "tkziBG",
                "to_list_item_id": "CEMVLw",
                "relationship": "Mother or Father"
            },
            {
                // Father's relationships to child 2
                "list_item_id": "tkziBG",
                "to_list_item_id": "uknZxD",
                "relationship": "Mother or Father"
            },
            {
                // Mother's relationship to child 1
                "list_item_id": "jBlqGM",
                "to_list_item_id": "CEMVLw",
                "relationship": "Mother or Father"
            },
            {
                // Mother's relationship to child 2
                "list_item_id": "jBlqGM",
                "to_list_item_id": "uknZxD",
                "relationship": "Mother or Father"
            },
            {
                // Child 1's relationship to child 2
                "list_item_id": "CEMVLw",
                "to_list_item_id": "uknZxD",
                "relationship": "Brother or Sister"
            }
        ]
    }
]
```

### Answer Array Example (Address type)

```json
"answers": [
  // Example of 2 address question answers
  {
    "answer_id": "other-address-uk-answer",
    "value": {
      "line1": "20 My Street",
      "line2": "Middleton",
      "town": "Mint Town",
      "postcode": "AB12 CD1",
      "uprn": "722100964321"

    }
  },
  {
    "answer_id": "workplace-address-answer",
    "value": {
      "line1": "55 Your Street",
      "line2": "Lowerton",
      "town": "Ice Town",
      "postcode": "XY12 VW1"
    }
  }
]
```

## Feedback Data

For a submission payload `type` of `uk.gov.ons.edc.eq:feedback` this will contain survey feedback properties with corresponding user entered values:

- `feedback_text`
- `feedback_type`
- `feedback_count`

### Feedback example

```json
"data": {
    "feedback_text": "I like this survey",
    "feedback_type": "Page design and structure",
    "feedback_count": "7"
}
```
