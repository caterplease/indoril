# REALMS OF INDORIL
## Naming Conventions

### Element Source
**Realms of Indoril**

### Element Id
> Prefix element Id's with the Realms of Indoril signature. The PHB signature is removed to distinguish from the core rulebooks and the homebrew.

`ID_ROI_`

### Feature Replacement Id
> Prefix replacement Id's with the same convention as other third-party and first-party content.

`ID_INTERNAL_FEATURE_REPLACEMENT`

### Supports Keywords
> For `select` elements, use a similar prefix as the `id` property.

`ID_ROI_`

### About feature displays
Certain features already exist, but the homebrew rules need to augment their wording.

- If the feature uses a variable _(e.g. Bardic Inspiration)_

> For features that have built in variables using the `<stat>` tags in their `<rules>` section, you can just update that statistic in an included feature addition. This is the case for **Bardic Inspiration** with the **Greater Inspiration** feature, as follows;

```
<element name="Greater Inspiration" type="Class Feature" source="Realms of Indoril" id="ID_ROI_CLASS_FEATURE_BARD_GREATER_INSPIRATION">
    <requirements>!ID_INTERNAL_FEATURE_REPLACEMENT_BARD_GREATER_INSPIRATION</requirements>
    <description>
        <p>At 11th level, you gain <strong>two additional</strong> uses of Bardic Inspiration.</p>
    </description>
    <sheet display="false" />
    <rules>
        <stat name="bardic-inspiration:count" value="2" level="11" />
    </rules>
</element>
```

- If the feature does _not_ use a variable _(e.g. Druid, Wild Shape)_

> For features that include a lot of text that needs to be augmented, or usage values that do not have variables already plugged in, it is more useful to use **Feature Replacement** by using the **grant** tag to add the `id` of the `<requirement>` replacement. An example of this is Druid's **Wild Shape**. 

First, the original Wild Shape is **replaced** in the actual top-level feat.

```
<element name="Realms of Indoril Rules (Druid)" type="Feat" source="Realms of Indoril" id="ID_ROI_RULES_DRUID">
    <supports>RULES_INDORIL_DRUID</supports>
    <requirements>!ID_ROI_RULES</requirements>
    <description>
        <p>Update your class to be compatible with the <strong>Realms of Indoril</strong> series of games. This will replace certain class features, add new ones, and augment existing ones.</p>
        <div element="ID_ROI_CLASS_FEATURE_DRUID_SPELLCASTING" />
        <div element="ID_ROI_CLASS_FEATURE_DRUID_BEAST_SPEECH" />
        <div element="ID_ROI_CLASS_FEATURE_DRUID_SWIFT_WILDSHAPE" />
        <div element="ID_ROI_CLASS_FEATURE_DRUID_GREATER_WILDSHAPE" />
        <div element="ID_ROI_CLASS_FEATURE_DRUID_PRIMAL_RENEWAL" />
        <div element="ID_ROI_CLASS_FEATURE_DRUID_EXPANDED_WILDSHAPE" />
    </description>
    <sheet display="false" />
    <rules>
        <!-- REPLACEMENTS -->
        <grant type="Replacement" id="ID_INTERNAL_FEATURE_REPLACEMENT_DRUID_WILD_SHAPE" />
        <!-- ADDITIONS -->
        ....

    </rules>
</element>
```

Then, the actual **Greater Wild Shape** feature is added, which replaces it.

```
<element name="Greater Wild Shape" type="Class Feature" source="Realms of Indoril" id="ID_ROI_CLASS_FEATURE_DRUID_GREATER_WILDSHAPE">
    <requirements>!ID_INTERNAL_FEATURE_REPLACEMENT_DRUID_GREATER_WILDSHAPE</requirements>
    <description>
        <p>Your Wild Shape is stronger, and continues to improve as you gain Druid levels. When you use Wild Shape, the maximum challenge rating of a beast you can transform into is shown on the Greater Wild Shape table.</p>
        ...
        <table class="class-features">
            ...
        </table>
    </description>
    <sheet action="Bonus Action" level="2" usage="{{druid wildshape:count}}/Short Rest">
        <description level="2">You can magically assume the shape of a beast that you have seen before.
        You can stay in a beast shape for {{druid wildshape:hours}} hours. You then revert to your normal form unless you expend another use of this feature. You can revert to your normal form earlier by using a bonus action on your turn. You automatically revert if you fall unconscious, drop to 0 hit points, or die.</description>
        <description level="20" usage="">You can magically assume the shape of a beast that you have seen before.
        You can stay in a beast shape for {{druid wildshape:hours}} hours. You then revert to your normal form unless you expend another use of this feature. You can revert to your normal form earlier by using a bonus action on your turn. You automatically revert if you fall unconscious, drop to 0 hit points, or die.</description>
    </sheet>
    <rules>
        <stat name="druid wildshape:hours" value="level:druid:half" />
        <stat name="druid wildshape:count" value="2" level="2" />
    </rules>
</element>
```

### For Variable Selections

Some homebrew features have conditions _(e.g. Cleric, Sacred Calling)_ where the options available depend on another option chosen.

> For `select` that uses only built in filtering _(e.g. Prepared Spells, Magical Secrets, etc)_ use the native, built in filters.

```
<rules>
    <select type="Spell" name="Magical Secrets" supports="(0)||($(spellcasting:slots))" number="2" level="10" spellcasting="Bard"/>
</rules>
```

> For `select` that uses custom filtering _(e.g. Cleric, Sacred Calling)_, you can copy existing options or create new ones, and use the `<supports>` tag to create the appropriate filters.

```
<!-- 
    DIVINE ACOLYTE
-->
<element name="Divine Acolyte" type="Class Feature" source="Realms of Indoril" id="ID_ROI_CLASS_FEATURE_CLERIC_SACRED_CALLING_DIVINE_ACOLYTE">
    <supports>Sacred Calling</supports>
    ...
    <rules>
        <select type="Proficiency" name="Skill Proficiency (Divine Acolyte)" supports="ID_ROI_SACRED_CALLING_CLERIC_ACOLYTE"/>
    </rules>
</element>
```
Then use the `supports` tag in each available option, according to the convention.
```
<element name="Religion" type="Proficiency" source="Realms of Indoril" id="ID_ROI_PROFICIENCY_SKILL_RELIGION">
    <supports>ID_ROI_SACRED_CALLING_CLERIC_ACOLYTE</supports>
    ...
</element>
<element name="History" type="Proficiency" source="Realms of Indoril" id="ID_ROI_CLERIC_SACRED_CALLING_DIVINE_ACOLYTE_PROFICIENCY_SKILL_HISTORY">
    <supports>ID_ROI_SACRED_CALLING_CLERIC_ACOLYTE</supports>
    ...
</element>
<element name="Insight" type="Proficiency" source="Realms of Indoril" id="ID_ROI_CLERIC_SACRED_CALLING_DIVINE_ACOLYTE_PROFICIENCY_SKILL_INSIGHT">
    <supports>ID_ROI_SACRED_CALLING_CLERIC_ACOLYTE</supports>
    ...
</element>
<element name="Medicine" type="Proficiency" source="Realms of Indoril" id="ID_ROI_CLERIC_SACRED_CALLING_DIVINE_ACOLYTE_PROFICIENCY_SKILL_MEDICINE">
    <supports>ID_ROI_SACRED_CALLING_CLERIC_ACOLYTE</supports>
    ...
</element>
<element name="Persuasion" type="Proficiency" source="Realms of Indoril" id="ID_ROI_CLERIC_SACRED_CALLING_DIVINE_ACOLYTE_PROFICIENCY_SKILL_PERSUASION">
    <supports>ID_ROI_SACRED_CALLING_CLERIC_ACOLYTE</supports>
    ...
</element>
```