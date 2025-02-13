<div class="stu-note">
This is IG is experimental and is authored for testing purposes. It is not intended to be used for genuine usage. See the <a href="https://build.fhir.org/ig/hl7au/au-fhir-ps/">AU Patient Summary Implementation Guide</a> for guidance on the generation of patient summaries in an Australian context.
</div>

### Introduction
This Implementation Guide is created as part of a tooling test to explore modelling options. It is not intended for further development or progression.

It shows different profiling approaches used to develop draft AU PS profiles, ensuring compliance with AU Core and IPS in alignment with the [Profile Design Principles for AU Patient Summary](https://confluence.hl7.org/display/HAFWG/Profile+Design+Principles+for+AU+Patient+Summary). The goal is to identify the most effective way to profile AU PS resources while maintaining conformance to national and international requirements.

We are testing different profiling strategies to assess the impact on:  
- how different derivation approaches align with AU Core and IPS
- the impact of using compliance declaration and imposed profiles
- whether the profiles are easy to interpret and apply
- whether FHIR tooling can process and enforce constraints
- ease of use for implementers, including whether a single implementation guide can provide all constraints reducing the need to reference AU Base, AU Core, and IPS separately


#### Relationship between HL7 AU and this implementation guide
* This implementation guide is published as a proof of concept implementation guide known to HL7 Australia.
* The content in this proof of concept guide may become an HL7 Australia specification.   
* This implementation guide is not endorsed by HL7 Australia or any of its members just by being made available via HL7 Australia or because it uses content from HL7 Australia specifications.

## Dependencies
{% include dependency-table.xhtml %}

### How to read this guide

This guide is divided into several pages which are listed at the top of each page in the menu bar.

- [Home](index.html): This page provides the introduction and scope for this guide.
- [FHIR Artefacts](artefacts.html): These pages provide detailed descriptions and formal definitions for the FHIR artefacts defined in this guide.
  - [Artefacts Summary](artifacts.html): This page lists the FHIR artefacts defined in this guide.
  - [Profiles and Extensions](profiles-and-extensions.html): This page describes the profiles and extensions that are defined in this guide. This page also defines the approach to testing out profiling, and describes what each profile is testing.
  - [Terminology](terminology.html): This is a placeholder for when or if content becomes available.
- [Examples](examples.html): This page lists all the examples used in this guide.
- [Support](downloads.html): These pages provide supporting material.
  - [Downloads](downloads.html): This page provides links to downloadable artefacts.
  - [License and Legal](license.html): This page outlines the license and legal requirements for material.