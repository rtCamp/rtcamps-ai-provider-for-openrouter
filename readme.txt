=== rtCamp’s AI Provider for OpenRouter ===
Contributors:      rtcamp, milindmore22, vishal4669, aishwarryapande, aviralmittal89
Tags:              ai, openrouter, llm, text-to-speech, image-generation
Requires at least: 7.0
Requires PHP:      7.4
Stable tag:        1.2.0
License:           GPL-2.0-or-later
License URI:       https://www.gnu.org/licenses/gpl-2.0.html
Tested up to:      7.1

OpenRouter connector for the WordPress AI Client.

== Description ==

rtCamp’s AI Provider for OpenRouter registers OpenRouter as an AI provider for the WordPress AI Client. It enables WordPress to access hundreds of AI models for text, image, and speech generation as well as decision making (including OpenAI GPT, Anthropic Claude, Google Gemini, and open-source models) through a single unified API and API key.

**Requirements:**

* **WordPress AI Client**: This plugin requires the [WordPress AI Client](https://wordpress.org/plugins/ai/) (`ai`) plugin (or core AI Client features in WordPress 7.0+) to be installed and active.
* **OpenRouter Account & API Key**: An API key from [OpenRouter](https://openrouter.ai/) is required to connect to models.

**Features:**

* OpenRouter provider registration for the WordPress AI Client.
* Configure different default models for text, image, text-to-speech (TTS), and decision models.
* Model selection displays live pricing (cost per 1M tokens, per image generation, or per request).
* Text generation using OpenRouter's chat completions endpoint, supporting all chat modalities.
* Image generation using OpenRouter's chat completions image modality.
* Text-to-speech (TTS) conversion using OpenRouter's audio speech endpoint.
* Dynamic voice selection populated based on the selected TTS model's supported voices.
* OpenRouter decision model support returning structured answers and calibrated probabilities for binary (noul), choice, scoring, and bounding box questions.
* Autocomplete model selection dynamically populated from OpenRouter's model catalog.

**Developer Filters:**

* `rtcamps_ai_provider_for_openrouter_text_generation_params`: Filter the request parameters before dispatching text generation requests to OpenRouter.
* `rtcamps_ai_provider_for_openrouter_image_generation_params`: Filter the request parameters before dispatching image generation requests to OpenRouter.
* `rtcamps_ai_provider_for_openrouter_tts_params`: Filter the request parameters before dispatching text-to-speech requests to OpenRouter.
* `rtcamps_ai_provider_for_openrouter_decision_params`: Filter the request parameters before dispatching decision model requests to OpenRouter.

== External services ==

This plugin connects to third-party services provided by OpenRouter, Inc. to retrieve available AI model listings and process AI generation requests.

* **Model Catalog Discovery:**
  * **What it is used for:** Fetching available text, image, speech, and decision models along with their pricing and voice metadata for the settings screen.
  * **What data is sent and when:** An HTTP GET request is sent to OpenRouter (`https://openrouter.ai/api/v1/models`) when an administrator visits the OpenRouter Settings page or queries the plugin's REST API endpoints (`/wp-json/rtcamps-ai-provider-for-openrouter/v1/models`, `/wp-json/rtcamps-ai-provider-for-openrouter/v1/image-models`, `/wp-json/rtcamps-ai-provider-for-openrouter/v1/tts-models`, and `/wp-json/rtcamps-ai-provider-for-openrouter/v1/decision-models`). No personal data, site data, or API keys are transmitted during this request. Retrieved models are cached locally in WordPress transients.
* **Content, Image, Speech Generation & Decision Making:**
  * **What it is used for:** Generating text completions, images, synthesizing speech, and evaluating structured decision questions requested through the WordPress AI Client and plugin APIs.
  * **What data is sent and when:** Prompts, context text/inputs, questions, and the configured OpenRouter API key (sent in the HTTP Authorization header) are sent via HTTPS POST to OpenRouter (`https://openrouter.ai/api/v1/chat/completions` for text and images, `https://openrouter.ai/api/v1/audio/speech` for speech synthesis, and `https://openrouter.ai/api/v1/systemone` or `https://openrouter.ai/api/alpha/decisions` for decision requests) only when a user or feature explicitly initiates an AI action.

This service is provided by OpenRouter, Inc.

* OpenRouter Terms of Service: [https://openrouter.ai/terms](https://openrouter.ai/terms)
* OpenRouter Privacy Policy: [https://openrouter.ai/privacy](https://openrouter.ai/privacy)

== Installation ==

1. Ensure the **WordPress AI Client** plugin (`ai`) is installed and activated.
2. Upload plugin files to `/wp-content/plugins/rtcamps-ai-provider-for-openrouter/` or install via the WordPress Plugins menu.
3. Activate **rtCamp’s AI Provider for OpenRouter** through the Plugins menu in WordPress.
4. Add your OpenRouter API key in **Settings > Connectors**.
5. Configure default text, image, text-to-speech, and decision models in **Settings > OpenRouter Settings**.

== Screenshots ==
1. Connector settings page showing OpenRouter API key configuration.
2. OpenRouter settings page showing model selection and cost information.
3. Example of generating post excerpt using OpenRouter in the WordPress editor.
4. Example of generating an image using OpenRouter in the WordPress editor.

== Frequently Asked Questions ==

= How do I get an OpenRouter API key? =

Visit [OpenRouter](https://openrouter.ai/) to create an account and generate an API key at https://openrouter.ai/settings/keys.

= Does this plugin work without the WordPress AI Client? =

No, this plugin requires the WordPress AI Client plugin (`ai` or `wordpress/php-ai-client`) to be installed and activated. It registers OpenRouter as an available provider for the WordPress AI Client.

= Where do I set the OpenRouter API key? =

Once the WordPress AI Client is installed, set your API key in **Settings > Connectors** under the OpenRouter section.

= Where do I choose the text, image, speech, and decision models? =

Use **Settings > OpenRouter Settings** to choose default models for text, image, text-to-speech, and decision models, as well as preferred voices.

= How much does it cost to generate text, images, and speech? =

The cost depends on the models you choose. The settings page displays the cost per 1M tokens for text and decision models, per image for image models, and pricing for speech models.

= Which OpenRouter models are selected by default? =

The plugin defaults to `openrouter/free` for text, `openrouter/auto` for images (as there are no free image options available), and `heygen/heygen-voice-1-turbo:free` for speech synthesis, but you can change these in the settings.

= Can I use text-to-speech (TTS) with OpenRouter? =

Yes. The plugin integrates with OpenRouter's audio speech API and the WordPress AI Client text-to-speech conversion interface. You can select any OpenRouter model supporting the speech modality (such as HeyGen Voice Turbo, OpenAI TTS, etc.) and choose from the model's supported voices.

= What are OpenRouter Decision Models and how do I use them? =

OpenRouter decision models evaluate typed questions about a piece of state and return calibrated probabilities and structured answers instead of freeform text. They support binary questions (`noul`), multiple choice, scores/ratings, and bounding box detections. You can configure a default decision model in **Settings > OpenRouter Settings** and access it programmatically via `OpenRouterProvider::decisionModel()`.

== Source Code ==

The source code is available on <a href="https://github.com/rtCamp/rtcamps-ai-provider-for-openrouter">GitHub</a>.

== Changelog ==

= 1.2.0 =
* Added support for Text-to-Speech (TTS) conversion via OpenRouter's `/api/v1/audio/speech` endpoint.
* Added settings to choose Default TTS Model and Voice with live autocomplete and dynamic voice listing.
* Set default TTS model to free model `heygen/heygen-voice-1-turbo:free`.
* Added support for OpenRouter Decision Models via `/api/v1/systemone` and `/api/alpha/decisions` endpoints, returning calibrated probabilities and structured answers (choice, noul/binary, score, and bounding box).
* Added setting to configure Default Decision Model with live autocomplete and catalog discovery.
* Added helper methods `OpenRouterProvider::decisionModel()`, `OpenRouterProvider::decisionModelIds()`, and `OpenRouterProvider::alphaUrl()`.
* Added REST API endpoints `/wp-json/rtcamps-ai-provider-for-openrouter/v1/tts-models` and `/wp-json/rtcamps-ai-provider-for-openrouter/v1/decision-models`.
* Added developer filters `rtcamps_ai_provider_for_openrouter_tts_params` and `rtcamps_ai_provider_for_openrouter_decision_params`.

= 1.1.1 =
* Renamed the plugin to "rtCamp’s AI Provider for OpenRouter" for better clarity and branding.
* Updated the plugin description to reflect the new name and provide a clearer overview of its functionality.
* Updated slug, constants, and text domain to match the new plugin name for consistency across the codebase.

= 1.1.0 =
* Updated overall settings page UI for better user experience and added tooltips for each setting to provide more context and guidance to users.
* Added badges to indicate the different cost of each type of request such as input, output, image read, web search, cache read, and cache write to help users make informed decisions when selecting models and generating content.
* Refactored code to convert plugins js files to TypeScript for better maintainability and type safety.
* Updated dependencies to ensure compatibility with the latest WordPress and AI plugin versions.

= 1.0.0 =

* Initial release of the OpenRouter provider plugin.
* Added text and image model settings.
* Added OpenRouter image generation support via chat completions image modality.

== Upgrade Notice ==
= 1.2.0 =
Added Text-to-Speech (TTS) conversion support with voice selection, OpenRouter Decision Model integration, and new REST endpoints.

= 1.1.1 =
Renamed the plugin to "rtCamp’s AI Provider for OpenRouter" for better clarity and branding.