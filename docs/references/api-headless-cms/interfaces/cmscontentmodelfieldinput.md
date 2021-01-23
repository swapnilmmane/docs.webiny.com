# Interface: CmsContentModelFieldInput

A definition for content model field received from the user.

Input type for `CmsContentModelField`.

**`see`** CmsContentModelField

## Hierarchy

* **CmsContentModelFieldInput**

## Table of contents

### Properties

- [fieldId](cmscontentmodelfieldinput.md#fieldid)
- [helpText](cmscontentmodelfieldinput.md#helptext)
- [id](cmscontentmodelfieldinput.md#id)
- [label](cmscontentmodelfieldinput.md#label)
- [listValidation](cmscontentmodelfieldinput.md#listvalidation)
- [multipleValues](cmscontentmodelfieldinput.md#multiplevalues)
- [placeholderText](cmscontentmodelfieldinput.md#placeholdertext)
- [predefinedValues](cmscontentmodelfieldinput.md#predefinedvalues)
- [renderer](cmscontentmodelfieldinput.md#renderer)
- [settings](cmscontentmodelfieldinput.md#settings)
- [type](cmscontentmodelfieldinput.md#type)
- [validation](cmscontentmodelfieldinput.md#validation)

## Properties

### fieldId

• **fieldId**: *string*

A unique ID for the field. Values will be mapped via this value.

___

### helpText

• `Optional` **helpText**: *string*

Text to display below the field to help user what to write in the field.

___

### id

• **id**: *string*

Generated ID.

___

### label

• **label**: *string*

Label for the field.

___

### listValidation

• **listValidation**: [*CmsContentModelFieldValidation*](cmscontentmodelfieldvalidation.md)[]

**`see`** CmsContentModelField.listValidation

___

### multipleValues

• `Optional` **multipleValues**: *boolean*

Are multiple values allowed?

___

### placeholderText

• `Optional` **placeholderText**: *string*

Text to display in the field.

___

### predefinedValues

• `Optional` **predefinedValues**: [*CmsContentModelFieldPredefinedValues*](cmscontentmodelfieldpredefinedvalues.md)

Predefined values options for the field. Check the reference for more information.

___

### renderer

• `Optional` **renderer**: CmsContentModelFieldRenderer

Renderer options for the field. Check the reference for more information.

___

### settings

• `Optional` **settings**: *Record*<*string*, *any*\>

User defined settings.

___

### type

• **type**: *string*

Type of the field. A plugin for the field must be defined.

**`see`** CmsModelFieldToGraphQLPlugin

___

### validation

• `Optional` **validation**: [*CmsContentModelFieldValidation*](cmscontentmodelfieldvalidation.md)[]

List of validations for the field.