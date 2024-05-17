# ⚙️ Config / Metadata

### The chub\_meta.yaml File

Every extension has a file at public/chub\_meta.yaml that contains extension metadata like its name, description, tags, et cetera. The file in the template ([https://github.com/CharHubAI/extension-template/blob/main/public/chub\_meta.yaml](https://github.com/CharHubAI/extension-template/blob/main/public/chub\_meta.yaml)) is annotated with explanations of each field.

### User Configurations

If you would like people to be able to configure your extension, for example, to set the size of a maze, you need to define a `config_schema` in your metadata yaml. This will allow the UI to generate a form for it.&#x20;

<figure><img src="../../.gitbook/assets/Screenshot 2024-04-29 at 15-46-32 A private chat with The Maze.png" alt="" width="188"><figcaption><p>A very simple generated config form.</p></figcaption></figure>

If you dislike yaml, you can instead list it as `config_schema: '@config.schema.json'` where the file is present in the `public` folder. An example schema with at least one example of everything supported is here: [https://github.com/CharHubAI/extension-integration-test/blob/main/public/config.schema.json](https://github.com/CharHubAI/extension-integration-test/blob/main/public/config.schema.json)&#x20;

The schema type is largely based around the JSON Schema specification (2020), with two caveats:

1. It does not, generally, support "advanced" features of the spec like 'anyOf', external references, field name patterns, et cetera. If you have a use case requiring this level of complexity in configuration, let us know so we can better understand the use case for this and support it.
2. If you give the "type" as "character\_map" and "value\_type" as any valid schema, the form will generate to get configuration information of that type for every character in the chat. For example, this is a valid config schema:

```
config_schema:
  # Will show in the UI.
  title: Expressions Config
  # Will show in the UI if not null or empty as a subtitle.
  description: 'Configurations for expression packs'
  type: "object"
  properties:
    selected:
      # The beginning of non-standard JSON Schema behavior.
      # JSchema isn't great with the concept of dynamic
      # schema generation. This is our little addition
      # to make it easy to define "I need this setting for each character".
      type: "character_map"
      # Could also be an object, etc.
      value_type:
        type: "string",
        enum: 'character.extensions.chub.alt_expressions.keys()'
        default: 'default'
      # For the type "character_map",
      # this is a boolean instead of the normal list.
      # If true, required for each character.
      required: false
      # Information shown beside this part of the form.
      description: "Which expression pack to use, if there are multiple."
  required: []
```

Even if you do have a configuration schema, it should be considered as optional, and the implementation of your extension constructor and load function should have handling for both the configuration being null and any given field in the configuration being null or undefined. As an example, here is the config schema for the maze extension:

```
config_schema:
  type: object
  title: Maze Configuration
  description: Settings for the maze extension.
  properties:
    size:
      title: "Maze Size"
      description: "The number of tiles wide (and tall) the maze is. You start in the center."
      type: integer
      minimum: 3
      maximum: 60
      default: 15
      multipleOf: 3
    imagePromptPrefix:
      type: string
      maxLength: 400
      title: "Image Prompt Prefix"
      description: "Experimental, may do nothing. A prefix to use for generated images."
      default: "Highest quality, 8K, digital art, "
  required:
    - size
    - imagePromptPrefix
```

Despite having 'required' fields, it still needs to handle the edge cases for missing and partial configurations in its constructor:

```
constructor(data: InitialData<InitStateType, ChatStateType, MessageStateType, ConfigType>) {
    super(data);
    // ... other initialization logic...
    this.size = config != null && config.hasOwnProperty('size') && config.size != null ? config.size : 15;
    if(config != null && config.hasOwnProperty('imagePromptPrefix') && config.imagePromptPrefix != null) {
        this.imagePromptPrefix = config.imagePromptPrefix;
    } else {
        this.imagePromptPrefix = 'Highest quality, 8K, digital art, ';
    }
}
```
