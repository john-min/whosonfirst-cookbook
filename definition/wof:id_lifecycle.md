# Who's on First ID Tracking and Life Cycle Documentation

## What is a `wof:id`?
A Who's on First ID (`wof:id`) is a unique 64-bit identifier that represents a single point or polygon feature in Who's on First. This identifier is commonly produced by the [Brooklyn Integers](https://www.brooklynintegers.com) service, though alternative methods of retrieving a `wof:id` are available. Unlike OpenStreetMap (OSM) or the Local Ordnance Survey (UK) a `wof:id` is stable to an individual feature and will not update when minor updates to a feature occur. Once a feature is given a `wof:id`, that feature will maintain that `wof:id` for it's entire lifecycle.

That's not to say that features never change; often times a feature is updated (increases in size, changes placetype, is given additional attributes, etc.) which may require a new feature to be created with a new `wof:id`. Updates to a feature that require a new feature with a new `wof:id` to be created are classified as "Significant Events". If changes fall under the Significant Event category, a new feature with new `wof:id` is created, then the non-valid feature is given a `wof:superseded_by` value equal to that of the new feature's `wof:id` and the new feature is given a `wof:supersedes` value equal to that of the non-valid feature. By linking features through the `supersedes` and `superseded_by` values, it allows a user and various map services to string together the history of any given feature and understand which features are no longer valid (and which features _are_ valid).

Who's on First is not in the business of removing features from history, but rather looks to take a snapshot in time and preserve features based on what _was_ and what _is_. Our `supersedes` and `superseded_by` fields allow us to look back to find features that existed at a certain time, with the help of those `supersedes` and `superseded_by` ID values.

Below, the life cycle and tracking rules are outlined to help you understand what changes require a new `wof:id` and what changes allow the `wof:id` to be kept as-is.

## Life Cycle / Tracking Rules

Feature life cycles and tracking rules are necessary in understanding the rules surrounding changes to the `wof:id`, what constitutes a change, and how Who's on First tracks both valid and non-valid features. By following the steps below, a standard is set in Who's on First to ensure that all users and mapping services are able to follow along with changes to a feature and the history of a given feature. 

Additionally, what Who's on First considers a Significant Event may be different than what a data user would consider a Significant Event. This could mean that while a feature in Who's on First was given a new record and `wof:id`, a specific user may not agree with that change - therefore, it is important for us to outline the rules and guidelines around Who's on First features and the `wof:id` field so users and their mapping services can optimize their data usage.

### wof:superseded and wof:superseded_by:

Keeping up with `wof:id` changes and new features taking the place of old, outdated features can be tricky business. Who's on First has a built-in series of attributes that can be used to track the changes and updates to a feature, even if a Significant Event has taken place and replaced a `wof:id`. 

For example, in 1991, the country of Yugoslavia began to break-up into countries that we now call Slovenia, Croatia, Bosnia and Herzegovina, the Republic of Macedonia, Montenegro and Serbia which includes (Vojvodina and Kosovo). Yugoslavia has it's own `wof:id`, but because it ceased to exist as a formal entity after 1991, it was given a cessation date in 1991 and superseded_by the new `wof:id`s of the six countries that came out of Yugoslavia. The six new countries were given new `wof:id`s, new geometries (portions of the former Yugoslavia geometry) and new attributes. They were also given a supersedes value of Yugoslavia's `wof:id`.

We'll refer to the non-valid feature as the **superseded** version and the new feature as the **superseding** version.

## Changing a `wof:id`

There are three reasons for changing a `wof:id`; these reasons include: 

- **Creation of a New Feature:** A new `wof:id` should be minted when a feature previously unknown to Who's on First is added to the Who's on First database.
- **Making Significant Modifications to an Existing Feature:** If specific, major modifications are made to an existing feature in Who's on First, a new feature with a new `wof:id` needs to be created. Superseding work is required to replace the old feature with the new feature.
- **Adding a deprecated or cessation date to an Existing Feature:** If an existing feature in Who's on First is found to have never been correct, either due to an error correction or real-world change, a new feature with a new `wof:id` needs to be created. Superseding work is required to replace the old feature with the new feature.

Each of these three reasons for changing a `wof:id` are explained below. 
 
### Creation of a New Feature:

Creating a new feature that was previously unknown to Who's on First is the most straightforward; no existing records need modifications and the only required work takes place in the record for the newly created feature. 

#### Examples

An example of a new feature would be a venue record for a new restaurant in New York City. If said restaurant was opened in September 2016, Who's on First would not known about this feature previous to this date, which means a new `wof:id` would be created and attached to a venue record in the New York venues repository for inclusion into Who's on First. Note that since this feature was never in Who's on First prior to the creation of this new venue record, a new `wof:id` would be minted from Brooklyn Integers.

Another example could be a new military facility on a Pacific Island. Similar to the new venue record described above, this facility would receive a new `wof:id`, geometry, and attributes. This feature is an example of a completely new feature to Who's on First.

### Making Significant Modifications to an Existing Feature

Significant modifications fall into one of two categories: _real-world changes_ or _error corrections_. Both of these categories can have one of two edits made: **geometry edits** or **attribute edits** (or both). The following changes, what Who's on First calls a Significant Event, would require a new `wof:id` to be minted for a superseding feature and updates to a superseded feature.

- Changes to the `wof:placetype`
- Changes to the geometry, where more than 50% (area or length) is added or removed
- Moving a feature's location more than (roughly) ten kilometers or five miles from it's original location

#### Examples

**Changes to the `wof:placetype`**

If Who's on First incorrectly classified a set of localities as regions, said region records would become the superseded records, with new superseding locality records created in their place. The `mz:is_current` field, `wof:superseded_by`, and `edtf:cessation` date field would be updated for each of the superseded region records. Completely new features with new `wof:id`s, a corrected `wof:placetype` field, and updated `wof:supersedes` field would be created for the superseding locality records. 

In this case, since the correction was made on the `wof:placetype` field and other attribute field values were correct, all other correct attributes would be transferred to the superseding feature (zoom levels, geometries, concordances, hierarchies, etc).

**Changes to the geometry, where more than 50% (area or length) is added or removed**

The Yugoslavia example, above, is a perfect example of a real-world geometry change causing new `wof:id`s to be minted. In this case, Yugoslavia was dissolved and split into several different countries (and a disputed area). The record for Yugoslavia would recieve a date (the date of it's dissolution) in the `edtf:cessation` field, an updated `mz:is_current` field, and the `wof:id`s of the newly created countires in it's `wof:superseded_by` field.

The new countries, Slovenia, Croatia, Bosnia and Herzegovina, the Republic of Macedonia, Montenegro and Serbia which includes (Vojvodina and Kosovo), would each recieve new `wof:id`s, and would have their `wof:supersedes` field updated to  include the `wof:id` of Yugoslavia. These would be our superseding features.

This superseding work would allow someone looking at, say, Montenegro, to see when it was created and what superseded feature it came from.

**Moving a feature's location more than (roughly) ten kilometers or five miles from it's original location**

Similar to the Yugoslavia example, above, if a feature has it's geometry move more than ten kilometers or five miles from its original location, a new feature with a new `wof:id` would be created. 

If an error correction needed to occur to move Iceland, for example, fifty kilometers to the east (let's pretend it was imported incorrectly), the record for Iceland would recieve a date (the date of it's error correction) in the `edtf:deprecated` field, an updated `mz:is_current` field, and the `wof:id` of the newly created Iceland record in it's `wof:superseded_by` field.

The new Iceland record would recieve a new `wof:id`, and would have it's `wof:supersedes` field updated to include the `wof:id` of the original Iceland record. This new record would be our superseding features.

### Adding a deprecated or cassation date to an existing feature

A superseded feature will either need a `edtf:deprecated` or `edtf:cessation` date field update. This date is an essential attribute, as it identifies the specific date at which that feature was superseded (and when the superseding feature took over as the valid feature). When one of these field updates occurs, the superseding feature will always get a new `wof:id`.

* `edtf:deprecated` - Used when a feature was **never** correct to begin with. _Typically_ the field to update when dealing with an error correction.

* `edtf:cessation` - Used when a feature was correct at a point in time, but for one or more reasons is no longer correct.

#### Examples

The `edtf:deprecated` field would be used, for example, when a set of features was imported with invalid geometries or inaccurate placetype designations.

Updates to the `edtf:cessation` date would occur, for example, when the Yugoslavia record was no longer valid. At one point in time, Yugoslavia was a valid feature in Who's on First. 

### `wof:id` Lifecycle Flowchart

![flowchart.png](https://cloud.githubusercontent.com/assets/18567700/18456781/81bb5c02-7908-11e6-98d4-048b23694a50.png)