This section shows different profiling approaches used to develop draft AU PS profiles, ensuring compliance with AU Core and IPS in alignment with the [Profile Design Principles for AU Patient Summary](https://confluence.hl7.org/display/HAFWG/Profile+Design+Principles+for+AU+Patient+Summary). The goal of this testing is to determine the most effective way to profile AU PS resources while maintaining conformance to national and international requirements.

We are evaluating different profiling strategies to determine the impact on conformance, consistency, implementation practicality, and ease of use for implementers, e.g.:  
- how different derivation approaches affect compliance with AU Core and IPS
- the impact of using compliance declaration and imposed profiles
- is the profile easy to interpret and apply
- tooling and validation support, i.e. can standard FHIR tooling process and enforce constraints
- ease of use for implementers, including whether a single implementation guide can provide a full set of constraints reducing the need to reference multiple implementation guides (AU Base, AU Core, IPS) separately
...to ensure that profiling decisions meet the technical conformance and also practical implementations needs..

FHIR provides two key mechanisms for ensuring compliance across different specifications: 
1. [StructureDefinition Complies With Profile (compliesWith)](https://hl7.org/fhir/extensions/StructureDefinition-structuredefinition-compliesWithProfile.html)
   - Used to declare that a profile complies with a given standard (e.g., AU Core, IPS). 
   - Allows flexibility since a profile does not need to be directly derived from AU Core or IPS but can still be declared compliant.
   - Declaring compliesWith AU Core and IPS does not apply rules automatically. It states the intent to comply, meaning the profile needs to already meet all AU Core and IPS constraints to be considered compliant. A profile can only be compliant if it explicitly applies AU Core and IPS constraints. 
1. [StructureDefinition Dependent Profiles (imposeProfile)](https://hl7.org/fhir/extensions/StructureDefinition-structuredefinition-imposeProfile.html)
   - Used to enforce conformance to another profile by ensuring the derived profile fully adopts the imposed constraints. 
   - More restrictive than [StructureDefinition Complies With Profile](https://hl7.org/fhir/extensions/StructureDefinition-structuredefinition-compliesWithProfile.html) in that it applies all constraints from the imposed profiles(s)
   - Used when a profile needs to inherit all rules of another profile without direct derivation


The table shows different profiling approaches for creating AU PS profiles. The table includes:
- the profile if and title, hyperlinked to the profile definition
- profile approach description that explains how the profile is derived and how it enforces AU Core and IPS constraints
- test scenarios, showing if the profile complies with AU Core, IPS, or both
- example instances used for testing

Example conventions used for testing
- the profile id, title and example instance include a suffix (e.g. -a)
- the only difference between instances is in the meta.profile
- the test results are visible in [qa.html](qa.html)
- example instances use single meta.profile declaration
  - declaring conformance to multiple AU PS had problems:
    - when validation passes, it is clear that the instance conforms to all profiles states in meta.profile
    - when validation fail, it is unclear which specific format it failed against
    - to ensure clarity, we declare conformance to a single profile per test scenario 


<table border="1" cellspacing="0" cellpadding="0" width="100%">
    <thead>
  <tr>
    <th>StructureDefinition id</th>
    <th>Title</th>
    <th>Profiling approach</th>
    <th>Test scenario</th>
    <th>Example id</th>
  </tr>
  </thead>
<tbody>
  <tr>
    <td rowspan="9">au-ps-medicationrequest-a</td>
    <td rowspan="9"><a href="StructureDefinition-au-ps-medicationrequest-a.html">AU PS MedicationRequest A</a></td>
    <td rowspan="9">Derive from AU Base, apply AU Core and IPS constraints, and state compliesWith AU Core and IPS.</td>
    <td rowspan="3">Compliant with IPS and AU Core</td>
    <td><a href="MedicationRequest-simvastatin-a-01.html">simvastatin-a-01</a></td>
  </tr>
  <tr>
    <td><a href="MedicationRequest-reaptan-01-a.html">reaptan-01-a</a></td>
  </tr>
  <tr>
    <td><a href="MedicationRequest-paracetamol-codeine-01-a.html">paracetamol-codeine-01-a</a></td>
  </tr>
  <tr>
    <td rowspan="3">Compliant with AU Core</td>
    <td><a href="MedicationRequest-simvastatin-02-a.html">simvastatin-02-a</a></td>
  </tr>
  <tr>
    <td><a href="MedicationRequest-reaptan-02-a.html">reaptan-02-a</a></td>
  </tr>
  <tr>
    <td><a href="MedicationRequest-paracetamol-codeine-02-a.html">paracetamol-codeine-02-a</a></td>
  </tr>
  <tr>
    <td rowspan="3">Compliant with IPS</td>
    <td><a href="MedicationRequest-simvastatin-03-a.html">simvastatin-03-a</a></td>
  </tr>
  <tr>
    <td><a href="MedicationRequest-reaptan-03-a.html">reaptan-03-a</a></td>
  </tr>
  <tr>
    <td><a href="MedicationRequest-paracetamol-codeine-03-a.html">paracetamol-codeine-03-a</a></td>
  </tr>
  <tr>
    <td rowspan="9">au-ps-medicationrequest-b</td>
    <td rowspan="9"><a href="StructureDefinition-au-ps-medicationrequest-b.html">AU PS MedicationRequest B</a></td>
    <td rowspan="9">Derive from AU Core, reapply AU Core constraints, apply IPS constraints, and state compliesWith IPS.</td>
    <td rowspan="3">Compliant with IPS and AU Core</td>
    <td><a href="MedicationRequest-simvastatin-01-b.html">simvastatin-01-b</a></td>
  </tr>
  <tr>
    <td><a href="MedicationRequest-reaptan-01-b.html">reaptan-01-b</a></td>
  </tr>
  <tr>
    <td><a href="MedicationRequest-paracetamol-codeine-01-b.html">paracetamol-codeine-01-b</a></td>
  </tr>
  <tr>
    <td rowspan="3">Compliant with AU Core</td>
    <td><a href="MedicationRequest-simvastatin-02-b.html">simvastatin-02-b</a></td>
  </tr>
  <tr>
    <td><a href="MedicationRequest-reaptan-02-b.html">reaptan-02-b</a></td>
  </tr>
  <tr>
    <td><a href="MedicationRequest-paracetamol-codeine-02-b.html">paracetamol-codeine-02-b</a></td>
  </tr>
  <tr>
    <td rowspan="3"><br>Compliant with IPS</td>
    <td><a href="MedicationRequest-simvastatin-03-b.html">simvastatin-03-b</a></td>
  </tr>
  <tr>
    <td><a href="MedicationRequest-reaptan-01-b.html">reaptan-01-b</a></td>
  </tr>
  <tr>
    <td><a href="MedicationRequest-paracetamol-codeine-03-b.html">paracetamol-codeine-03-b</a></td>
  </tr>
  <tr>
    <td rowspan="9">au-ps-medicationrequest-c</td>
    <td rowspan="9"><a href="StructureDefinition-au-ps-medicationrequest-c.html">AU PS MedicationRequest C</a></td>
    <td rowspan="9">Derive from AU Core, apply IPS constraints, and state compliesWith IPS.</td>
    <td rowspan="3">Compliant with IPS and AU Core</td>
    <td><a href="MedicationRequest-simvastatin-01-c.html">simvastatin-01-c</a></td>
  </tr>
  <tr>
    <td><a href="MedicationRequest-reaptan-01-c.html">reaptan-01-c</a></td>
  </tr>
  <tr>
    <td><a href="MedicationRequest-paracetamol-codeine-01-c.html">paracetamol-codeine-01-c</a></td>
  </tr>
  <tr>
    <td rowspan="3">Compliant with AU Core</td>
    <td><a href="MedicationRequest-simvastatin-02-c.html">simvastatin-02-c</a></td>
  </tr>
  <tr>
    <td><a href="MedicationRequest-reaptan-02-c.html">reaptan-02-c</a></td>
  </tr>
  <tr>
    <td><a href="MedicationRequest-paracetamol-codeine-02-c.html">paracetamol-codeine-02-c</a></td>
  </tr>
  <tr>
    <td rowspan="3">Compliant with IPS</td>
    <td><a href="MedicationRequest-simvastatin-03-c.html">simvastatin-03-c</a></td>
  </tr>
  <tr>
    <td><a href="MedicationRequest-reaptan-03-c.html">reaptan-03-c</a></td>
  </tr>
  <tr>
    <td><a href="MedicationRequest-paracetamol-codeine-03-c.html">paracetamol-codeine-03-c</a></td>
  </tr>
  <tr>
    <td rowspan="9">au-ps-medicationrequest-d</td>
    <td rowspan="9"><a href="StructureDefinition-au-ps-medicationrequest-d.html">AU PS MedicationRequest D</a></td>
    <td rowspan="9">Derive from FHIR, apply AU Base terminologies and identifiers, then apply AU Core and IPS constraints, and state compliesWith AU Core and IPS.</td>
    <td rowspan="3">Compliant with IPS and AU Core</td>
    <td><a href="MedicationRequest-simvastatin-01-d.html">simvastatin-01-d</a></td>
  </tr>
  <tr>
    <td><a href="MedicationRequest-reaptan-01-d.html">reaptan-01-d</a></td>
  </tr>
  <tr>
    <td><a href="MedicationRequest-paracetamol-codeine-01-d.html">paracetamol-codeine-01-d</a></td>
  </tr>
  <tr>
    <td rowspan="3">Compliant with AU Core</td>
    <td><a href="MedicationRequest-simvastatin-02-d.html">simvastatin-02-d</a></td>
  </tr>
  <tr>
    <td><a href="MedicationRequest-reaptan-02-d.html">reaptan-02-d</a></td>
  </tr>
  <tr>
    <td><a href="MedicationRequest-paracetamol-codeine-02-d.html">paracetamol-codeine-02-d</a></td>
  </tr>
  <tr>
    <td rowspan="3">Compliant& with IPS</td>
    <td><a href="MedicationRequest-simvastatin-03-d.html">simvastatin-03-d</a></td>
  </tr>
  <tr>
    <td><a href="MedicationRequest-reaptan-03-d.html">reaptan-03-d</a></td>
  </tr>
  <tr>
    <td><a href="MedicationRequest-paracetamol-codeine-03-d.html">paracetamol-codeine-03-d</a></td>
  </tr>
  <tr>
    <td rowspan="9">au-ps-medicationrequest-e</td>
    <td rowspan="9"><a href="StructureDefinition-au-ps-medicationrequest-e.html">AU PS MedicationRequest E</a></td>
    <td rowspan="9">Derived from IPS and constrained with AU Core rules where possible, without modifying IPS defined references, as changing references to different profiles is not allowed by the IG Publisher.</td>
    <td rowspan="3">Compliant with IPS and AU Core</td>
    <td><a href="MedicationRequest-simvastatin-01-e.html">simvastatin-01-e</a></td>
  </tr>
  <tr>
    <td><a href="MedicationRequest-reaptan-01-e.html">reaptan-01-e</a></td>
  </tr>
  <tr>
    <td><a href="MedicationRequest-paracetamol-codeine-01-e.html">paracetamol-codeine-01-e</a></td>
  </tr>
  <tr>
    <td rowspan="3">Compliant with AU Core</td>
    <td><a href="MedicationRequest-simvastatin-02-e.html">simvastatin-02-e</a></td>
  </tr>
  <tr>
    <td><a href="MedicationRequest-reaptan-02-e.html">reaptan-02-e</a></td>
  </tr>
  <tr>
    <td><a href="MedicationRequest-paracetamol-codeine-02-e.html">paracetamol-codeine-02-e</a></td>
  </tr>
  <tr>
    <td rowspan="3">Compliant with IPS</td>
    <td><a href="MedicationRequest-simvastatin-03-e.html">simvastatin-03-e</a></td>
  </tr>
  <tr>
    <td><a href="MedicationRequest-reaptan-03-e.html">reaptan-03-e</a></td>
  </tr>
  <tr>
    <td><a href="MedicationRequest-paracetamol-codeine-03-e.html">paracetamol-codeine-03-e</a></td>
  </tr>
  <tr>
    <td rowspan="9">au-ps-medicationrequest-f</td>
    <td rowspan="9"><a href="StructureDefinition-au-ps-medicationrequest-f.html">AU PS MedicationRequest F</a></td>
    <td rowspan="9">Derive from AU Core and use imposeProfile to enforce IPS constraints.</td>
    <td rowspan="3">Compliant with IPS and AU Core</td>
    <td><a href="MedicationRequest-paracetamol-simvastatin-01-f.html">simvastatin-01-f</a></td>
  </tr>
  <tr>
    <td><a href="MedicationRequest-reaptan-01-f.html">reaptan-01-f</a></td>
  </tr>
  <tr>
    <td><a href="MedicationRequest-paracetamol-codeine-01-f.html">paracetamol-codeine-01-f</a></td>
  </tr>
  <tr>
    <td rowspan="3"><br>Compliant with AU Core</td>
    <td><a href="MedicationRequest-simvastatin-02-f.html">simvastatin-02-f</a></td>
  </tr>
  <tr>
    <td><a href="MedicationRequest-reaptan-02-f.html">reaptan-02-f</a></td>
  </tr>
  <tr>
    <td><a href="MedicationRequest-paracetamol-codeine-02-f.html">paracetamol-codeine-02-f</a></td>
  </tr>
  <tr>
    <td rowspan="3">Compliant with IPS</td>
    <td><a href="MedicationRequest-simvastatin-03-f.html">simvastatin-03-f</a></td>
  </tr>
  <tr>
    <td><a href="MedicationRequest-reaptan-03-f.html">reaptan-03-f</a></td>
  </tr>
  <tr>
    <td><a href="MedicationRequest-paracetamol-codeine-03-f.html">paracetamol-codeine-03-f</a></td>
  </tr>
  <tr>
    <td rowspan="9">au-ps-medicationrequest-g</td>
    <td rowspan="9"><a href="StructureDefinition-au-ps-medicationrequest-g.html">AU PS MedicationRequest G</a> </td>
    <td rowspan="9">Derive from IPS and use imposeProfile to enforce AU Core constraints.</td>
    <td rowspan="3">Compliant with IPS and AU Core</td>
    <td><a href="MedicationRequest-simvastatin-01-g.html">simvastatin-01-g</a></td>
  </tr>
  <tr>
    <td><a href="MedicationRequest-reaptan-01-g.html">reaptan-01-g</a></td>
  </tr>
  <tr>
    <td><a href="MedicationRequest-paracetamol-codeine-01-g.html">paracetamol-codeine-01-g</a></td>
  </tr>
  <tr>
    <td rowspan="3">Compliant with AU Core</td>
    <td><a href="MedicationRequest-simvastatin-02-g.html">simvastatin-02-g</a></td>
  </tr>
  <tr>
    <td><a href="MedicationRequest-reaptan-02-g.html">reaptan-02-g</a></td>
  </tr>
  <tr>
    <td><a href="MedicationRequest-paracetamol-codeine-02-g.html">paracetamol-codeine-02-g</a></td>
  </tr>
  <tr>
    <td rowspan="3">Compliant with IPS</td>
    <td><a href="MedicationRequest-simvastatin-03-g.html">simvastatin-03-g</a></td>
  </tr>
  <tr>
    <td><a href="MedicationRequest-reaptan-03-g.html">reaptan-03-g</a></td>
  </tr>
  <tr>
    <td><a href="MedicationRequest-paracetamol-codeine-03-g.html">paracetamol-codeine-03-g</a></td>
  </tr>
  <tr>
    <td rowspan="9">au-ps-medicationrequest-h</td>
    <td rowspan="9"><a href="StructureDefinition-au-ps-medicationrequest-h.html">AU PS MedicationRequest H</a></td>
    <td rowspan="9">Derive from AU Base and use imposeProfile to enforce both AU Core and IPS constraints.</td>
    <td rowspan="3">Compliant with IPS and AU Core</td>
    <td><a href="MedicationRequest-simvastatin-01-h.html">simvastatin-01-h</a></td>
  </tr>
  <tr>
    <td><a href="MedicationRequest-reaptan-01-h.html">reaptan-01-h</a></td>
  </tr>
  <tr>
    <td><a href="MedicationRequest-paracetamol-codeine-01-h.html">paracetamol-codeine-01-h</a></td>
  </tr>
  <tr>
    <td rowspan="3">Compliant with AU Core</td>
    <td><a href="MedicationRequest-simvastatin-02-h.html">simvastatin-02-h</a></td>
  </tr>
  <tr>
    <td><a href="MedicationRequest-reaptan-02-h.html">reaptan-02-h</a></td>
  </tr>
  <tr>
    <td><a href="MedicationRequest-paracetamol-codeine-02-h.html">paracetamol-codeine-02-h</a></td>
  </tr>
  <tr>
    <td rowspan="3">Compliant with IPS</td>
    <td><a href="MedicationRequest-simvastatin-03-h.html">simvastatin-03-h</a></td>
  </tr>
  <tr>
    <td><a href="MedicationRequest-reaptan-03-h.html">reaptan-03-h</a></td>
  </tr>
  <tr>
    <td><a href="MedicationRequest-paracetamol-codeine-03-h.html">paracetamol-codeine-03-h</a></td>
  </tr>
</tbody>
</table>

### Profiles
<!-- ================================================ -->
<!--  use this line to include an autogenerated list of all profiles and highlight new ones using the input/data/new_stuff.yml list.  Remove it if you would like to hand generate it -->

{% include sd-list-generator.md %}
<!-- ================================================ -->

TBD

### Extensions

All extensions used in this guide are defined in the FHIR Extensions Pack or [AU Base](http://build.fhir.org/ig/hl7au/au-fhir-base/profiles-and-extensions.html#extensions).

The following extension are marked with Must Support in this implementation guide:

TBD