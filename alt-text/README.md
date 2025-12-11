# Alt Text

* **Type:** Text Generation
* **Tested With:**
  * Gemini 3 Pro
* **Includes:**
  * Meta-prompting
  * Role Prompting
  * Chain-of-Thought
  * Multimodal Prompting
  * Few-shot prompting

This is a multimodal prompt, taking in this system prompt (as text) and a relevant image and outputting JSON. The actual prompt is part of a little CMS I built for myself for [my photography site](https://snugug.photography/), so it returns both alt text and some potential titles for the image. You can also include capture metadata (which I do as JSON stringified EXIF data), geolocation data (which I do as a stringified lat/long object), and keywords (which I do as a stringified array). Always check the results, alt text is meant for humans, not robots.

## How it was made

Alt text always felt like a good candidate for generative AI as it combines two foundational machine learning concepts: image recognition and prose generation, where the prose generation can be grounded in the image recognition results. As such this started life as a basic Role & Goal prompt, but the results I got sometimes showed the wrong thing, sometimes was unsure of itself ("either this or that"), and sometimes rambled. When I went to revise it, I rewrote the role to be much stronger, focusing on curation and accessibility, and added specific requirements around what it should and shouldn't do, and then used meta-prompting to go back and turn it into a prompt. I then tested it with some of my weirder photographs, and went back and forth with my GenAI tool having it make adjustments to the prompt as I tested.

I actually ran into some trouble trying to fine-tune this prompt: in order to get it to correctly understand specific techniques like infrared or long exposure, I started passing it EXIF data. The output then became too over-reliant on that data and started putting specific portions of it (like f-stop or shutter speed) directly into the titles or alt text, so I needed to include specific constraints to tell it to _not_ do that.



## Why it works

* Because image recognition is a game of probabilities, there's a lot in this prompt meant to focus the output on just the highest likelihood results ("iconic or unmistakable landmarks", "Avoid all inference phrases", a whole constraints on inference), with geolocation data used for grounding.
* There is also a specific requirement on how to start the alt text—without it it almost always returned "This is an image of" or something like that at the start.
* Getting consistent JSON output from GenAI models is tough—they'll often wrap it in a Markdown code block. There is not one, not two, not three but _four_ different places in this prompt that explains what the output should look like, including a full sample JSON object.

## Sample images

Each sample includes the image and its generated alt text. Click on the images to see the full image page, including metadata, geolocation, and tags.

### Gemini 3

[![Neon Kobe Spires](https://snugug.photography/images/kobe-fuji-tokyo-2025/cuts/DSCF7820.2000.webp)](https://snugug.photography/kobe-fuji-tokyo-2025/neon-kobe-spires)

```
The iconic Kobe Port Tower, a brightly illuminated hyperboloid lattice structure, glows vibrantly in purple and blue against a deep indigo night sky. To its right, the distinctive, angular white lattice framework of the Kobe Maritime Museum gleams with cool blue light. In the foreground, a wide, paved plaza reveals several indistinct, blurred figures, a result of the photograph's long exposure. Behind these architectural landmarks, numerous high-rise buildings display warm yellow and white window lights, adding to the urban evening scene.
```

[![Orange Gaze](https://snugug.photography/images/kobe-fuji-tokyo-2025/cuts/DSCF7893.2000.webp)](https://snugug.photography/kobe-fuji-tokyo-2025/orange-gaze)

```
A small, white-faced owl with prominent gray ear tufts stares directly forward, its large, intense orange eyes captivating the viewer. Its plumage displays a sophisticated gradient of finely textured white and various shades of gray, creating intricate patterns across its head and body. A subtle yellow beak is partially visible amidst its facial feathers. The background is rendered with an extremely shallow depth of field, producing a soft, ethereal bokeh of blurred green foliage, cool blue hues, and hints of muted red.
```

[![Spectral, Eternal Fuji](https://snugug.photography/images/kobe-fuji-tokyo-2025/cuts/DSCF8215.2000.webp)](https://snugug.photography/kobe-fuji-tokyo-2025/spectral-eternal-fuji)

```
A stark monochromatic infrared photograph captures the iconic Mount Fuji, its upper slopes and distinct conical peak blanketed in luminous white snow. Below the majestic mountain, a dense, dark band of evergreen trees stretches across the mid-ground. In the foreground, a tranquil body of water reflects sparse reeds and dark, textured ground cover, all rendered in shades of grey. The clear sky overhead is a uniform light grey, contrasting with the bright, almost glowing natural elements below.
```