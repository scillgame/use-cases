# SCILL Use Cases

These JSON documents define a set of SCILL objects created via script or the Admin Panel that will set up an example application.

## JSON document structure

The JSON documents structure defines the objects to be created in your SCILL GaaS account. It will look like this:

```JSON
{
  "app": {
    "app_name": "Website",
    "app_tag": "website",
    "app_active": true
  },
  "apiKey": {
    "label": "Webhooks - do not delete"
  },
  "challengeCategories": [
    {
      "app_id": "__APP_ID__",
      "category_type": "__DAILY__CATEGORY_TYPE_ID__",
      "category_name": "Daily Challenges",
      "active": true
    },
    {
      "app_id": "__APP_ID__",
      "category_type": "__GENERIC__CATEGORY_TYPE_ID__",
      "category_name": "Profile",
      "active": true
    }
  ],
  "challenges": [
    {
      "app_id": "__APP_ID__",
      "challenge_category_id": "__CATEGORY_ID_INDEX_0__",
      "challenge_name": "Visit the website",
      "challenge_type": "craft-item",
      "challenge_description": "",
      "is_battle_pass_challenge": false,
      "challenge_reward_type_id": "__NOTHING_CHALLENGE_REWARD_TYPE_ID__",
      "repeatable": true,
      "challenge_icon": "",
      "challenge_icon_hd": "",
      "challenge_duration_time": 30,
      "challenge_goal": 5,
      "challenge_price": 0,
      "challenge_xp": 10,
      "challenge_reward": "",
      "challenge_goal_condition": 0,
      "challenge_auto_activated": true,
      "period_range": "i",
      "period_range_reset_at_time": "00:00:00",
      "period_range_time_zone": "Arctic/Longyearbyen",
      "generic_metadata": [
        {
          "values": ["page_impression"],
          "conjunction": "OR",
          "metadata_key": "item_type"
        }
      ]
    }
  ],
  "battlePassChallenges": [
    {
      "app_id": "__APP_ID__",
      "challenge_name": "Collect 100 Points",
      "challenge_type": "collect-item",
      "challenge_icon": "",
      "challenge_icon_hd": "",
      "challenge_goal": 5,
      "challenge_price": 0,
      "challenge_xp": 0,
      "challenge_goal_condition": 0,
      "generic_metadata": [
        {
          "values": ["points"],
          "conjunction": "OR",
          "metadata_key": "item_type"
        }
      ]
    }
  ],
  "battlePasses": [
    {
      "app_id": "__APP_ID__",
      "battle_pass_name": "User Levels",
      "start_date": "2021-01-01T00:00:00Z",
      "end_date": "2030-12-31T23:59:59Z",
      "is_unlocked_incrementally": true,
      "is_active": true,
      "levels": [
        {
          "app_id": "__APP_ID__",
          "battle_pass_id": "__BATTLE_PASS_ID__",
          "is_active": true,
          "reward_type_id": "__ITEM_BATTLE_PASS_REWARD_TYPE_ID__",
          "reward_amount": "",
          "challenges": [
            {
              "challenge_id": "__BATTLE_PASS_CHALLENGE_ID_INDEX_0__"
            }
          ]
        }
      ]
    }
  ]
}
```

Each section in this JSON document represents a content type within the SCILL platform. The objects defined for these properties are payloads which can be directly used when calling the `POST` requests in the REST-APIs or the Admin SDKs.

Some properties need to be replaced with real values from objects created before. To implement that, use these special values which will be replaced during the process. Please also make sure, that you leave the order intact.

|Variable|Description|
|--------|-----------|
|`__APP_ID__`| The id of the app created in the first step|
|`__BATTLE_PASS_ID__`| The id of the battle pass created in the first step|
|`__DAILY__CATEGORY_TYPE_ID__`| Right now, three different category types exist, one is the daily type|
|`__GENERIC__CATEGORY_TYPE_ID__`| Right now, three different category types exist, one is the generic type|
|`__TUTORIAL__CATEGORY_TYPE_ID__`| Right now, three different category types exist, one is the tutorial type|
|`__CATEGORY_ID_INDEX_0__`| The first index of the category created earlier|
|`__CATEGORY_ID_INDEX_n__`| The nth index of the category created earlier|
|`__CATEGORY_ID_INDEX_n__`| The nth index of the category created earlier|
|`__NOTHING_CHALLENGE_REWARD_TYPE_ID__`| The id of the "nothing" reward type|
|`__ITEM_CHALLENGE_REWARD_TYPE_ID__`| The id of the "item" reward type|
|`__EXPERIENCE_CHALLENGE_REWARD_TYPE_ID__`| The id of the "experience" reward type|
|`__MONEY_CHALLENGE_REWARD_TYPE_ID__`| The id of the "money" reward type|
|`__BATTLE_PASS_CHALLENGE_ID_INDEX_0__`| The id of the first battle pass challenge created earlier|
|`__BATTLE_PASS_CHALLENGE_ID_INDEX_n__`| The id of the nth battle pass challenge created earlier|
|`__NOTHING_BATTLE_PASS_REWARD_TYPE_ID__`| The id of the "nothing" reward type|
|`__ITEM_BATTLE_PASS_REWARD_TYPE_ID__`| The id of the "item" reward type|
|`__EXPERIENCE_BATTLE_PASS_REWARD_TYPE_ID__`| The id of the "experience" reward type|
|`__MONEY_BATTLE_PASS_REWARD_TYPE_ID__`| The id of the "money" reward type|

This is the process of the script:

1. First, the route will load the reward and category types and will create the "template" variables listed above.
2. Next, the route will iterate through each property of the JSON document, will replace all variables with actual values and will use the payload to create the SCILL object by using the objects REST-API endpoint.
3. The objects id (i.e. battle_pass_id, app_id) will be extracted from the response and the "template" variable will be set. For lists, a "template" variable with `_INDEX_n__` will be created so that it can be used in subsequent requests.
