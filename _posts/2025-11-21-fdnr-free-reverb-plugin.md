---
layout: default
title: "FDNR: A Free, Open-Source Reverb Plugin"
date: 2025-11-21 12:00:00 -0000
categories: vst plugins music-production
---

![FDNR Screenshot](https://github.com/stancsz/fdnr-vst3/raw/main/docs/screenshot.png)

If you are looking for a high-quality reverb plugin but don't want to break the bank, you might want to check out **FDNR (Feedback Delay Network Reverberator)**.

It is a **free, open-source plugin** that is similar to Valhalla reverbs (like VintageVerb and Room) but comes with a much more feature-rich engine under a modern, flat design. Best of all, it is totally free!

One of the coolest features is that you can share your reverb settings with your co-producers or friends simply by sharing a `.json` file. This makes collaboration and preset sharing incredibly easy.

You can download the plugin from the **[Releases Page](https://github.com/stancsz/fdnr-vst3/releases)**.

---

## What is FDNR?

**Feedback Delay Network Reverberator (FDNR)** is an open-source, modular reverb engine offering high-fidelity algorithms inspired by classic space reverberators. It features deep modulation, multiple reverb modes, and VST3 compatibility.

### Technical Deep Dive: Inside the FDN

At its core, FDNR relies on a **Feedback Delay Network (FDN)**. Unlike simple comb filter reverbs, an FDN uses a unitary matrix to mix the outputs of multiple delay lines before feeding them back into the inputs. This creates a dense, colorless tail that builds up echo density rapidly over time.

The signal flow can be visualized as follows:

1.  **Input**: The audio signal is distributed to $N$ parallel delay lines.
2.  **Delay Lines**: Each line delays the signal by a specific amount (determined by the mode/prime numbers).
3.  **Absorption Filters**: Low-pass and high-pass filters in the feedback loop simulate air absorption and material damping.
4.  **Feedback Matrix**: A Householder or Hadamard matrix mixes the delay line outputs. This mixing is crucial for maximizing echo density and preventing metallic ringing.
5.  **Output**: The tapped signals are summed to create the wet reverb signal.

<div class="mermaid">
graph LR
    Input[Input Signal] --> Split(( ))
    Split --> D1[Delay Line 1]
    Split --> D2[Delay Line 2]
    Split --> D3[Delay Line 3]
    Split --> D4[Delay Line 4]

    D1 --> Filter1[Absorption Filter]
    D2 --> Filter2[Absorption Filter]
    D3 --> Filter3[Absorption Filter]
    D4 --> Filter4[Absorption Filter]

    Filter1 --> Matrix[Feedback Matrix]
    Filter2 --> Matrix
    Filter3 --> Matrix
    Filter4 --> Matrix

    Matrix --> |Feedback| D1
    Matrix --> |Feedback| D2
    Matrix --> |Feedback| D3
    Matrix --> |Feedback| D4

    Matrix --> Output((Sum Output))
</div>

### Echo Density & Impulse Response

One of the key characteristics of an FDN is its rapidly increasing echo density. In the early reflections, you can hear distinct echoes (represented by the spikes in the graph below). As time progresses, the feedback matrix smears these echoes into a smooth, diffuse tail.

<div class="mermaid">
xychart-beta
    title "Conceptual Impulse Response of an FDN"
    x-axis "Time (ms)" [0, 50, 100, 150, 200, 250, 300]
    y-axis "Amplitude" [0, 1]
    line [1.0, 0.8, 0.4, 0.2, 0.1, 0.05, 0.02]
    bar [1.0, 0.6, 0.5, 0.3, 0.2, 0.1, 0.05]
</div>

*Note: The bar chart represents early reflections, while the line approximates the energy decay envelope (RT60).*

### Features

*   **21 Unique Reverb Modes**: Ranging from fast echoes to massive lush spaces and looping delays.
*   **Modular DSP Chain**:
    *   **Pre-Delay**: Up to 2000ms with modulation.
    *   **Warp**: Controls the modulation feedback and character.
    *   **Reverb Core**: Feedback Delay Network (FDN) based reverb with feedback and density controls.
    *   **EQ**: Low and High cut filters to shape the tone.
*   **Deep Modulation**: Adjustable Rate and Depth for chorus-like textures or pitch-shifting tails.
*   **Custom UI**: Dark, flat design inspired by classic hardware and software units.

### Controls

*   **MIX**: Controls the balance between the dry and wet signal.
*   **DELAY**: Sets the pre-delay time (0-1000ms).
*   **FEEDBACK**: Controls the decay time of the reverb tail.
*   **WIDTH**: Adjusts the stereo width of the output.
*   **WARP**: Adds modulation feedback and coloration.
*   **DENSITY**: Controls the density/diffusion of the reverb reflections.
*   **MOD RATE**: Sets the speed of the modulation LFO.
*   **MOD DEPTH**: Sets the intensity of the modulation.
*   **EQ HIGH/LOW**: Cuts high or low frequencies from the reverb tail.

### Algorithms (Modes)

FDNR offers a wide variety of algorithms to suit different needs:

*   **Twin Star**: Fast attack, shorter decay, high echo density.
*   **Sea Serpent**: Fast-ish attack, shorter decay, varying density.
*   **Horse Man**: Medium attack, longer decay, medium-high density.
*   **Archer**: Slow attack, longer decay, high density.
*   **Void Maker**: Medium attack, very long decay, massive spaces.
*   **Galaxy Spiral**: Slowest attack, very long decay, very high density.
*   **Harp String, Goat Horn, Nebula Cloud, Triangle**: Various combinations of attack, decay, and echo patterns.
*   **Cloud Major/Minor**: Strange repeating patterns with low density.
*   **Queen Chair/Hunter Belt**: Low initial density building to massive reverbs.
*   **Water Bearer/Two Fish**: EchoVerb algorithms with audible delays morphing into reverb.
*   **Scorpion Tail, Balance Scale, Lion Heart, Maiden, Seven Sisters**: Complex feedback and filtering networks.

## Installation

### For Users

1.  Download the latest Release for your operating system (Windows, Mac, or Linux) from the [Releases page](https://github.com/stancsz/fdnr-vst3/releases).
2.  Unzip the downloaded `.zip` file.
3.  **VST3 Plugin**:
    *   Find the `FDNR.vst3` file (or folder).
    *   Copy it to your system's VST3 directory:
        *   **Windows**: `C:\Program Files\Common Files\VST3\`
        *   **Mac**: `/Library/Audio/Plug-Ins/VST3/`
        *   **Linux**: `~/.vst3/` or `/usr/lib/vst3/` (or your DAW's VST3 folder)
4.  **Standalone Application (Optional)**:
    *   Find the FDNR executable (or app bundle).
    *   You can run this directly or copy it to your Applications folder.

**If the plugin doesn't appear in your host:**
*   Confirm the `.vst3` file is in a folder the DAW scans.
*   Rescan or restart the host application.
*   Check file permissions (macOS/Linux) and architecture (x64 vs arm64) compatibility.

---

## References

If you are interested in the math behind FDNs, check out these resources:

1.  **Jot, J.-M., & Chaigne, A. (1991).** *Digital delay networks for designing artificial reverberators.* Audio Engineering Society Convention 90.
2.  **Smith, J. O. (online).** *Physical Audio Signal Processing: Feedback Delay Networks (FDN).* [ccrma.stanford.edu](https://ccrma.stanford.edu/~jos/pasp/Feedback_Delay_Networks_FDN.html)
3.  **Schlecht, S. J., & Habets, E. A. P. (2019).** *Modal Decomposition of Feedback Delay Networks.* IEEE Transactions on Signal Processing.

---

Check out the source code on [GitHub](https://github.com/stancsz/fdnr-vst3).
