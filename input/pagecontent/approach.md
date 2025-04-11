### Testing out profiling approaches
This section shows different profiling approaches used to develop draft AU PS profiles, ensuring compliance with AU Core and IPS in alignment with the [Profile Design Principles for AU Patient Summary](https://confluence.hl7.org/display/HAFWG/Profile+Design+Principles+for+AU+Patient+Summary). The goal is to identify the most effective way to profile AU PS resources while maintaining conformance to national and international requirements.

We are testing different profiling strategies to assess the impact on:  
- how different derivation approaches align with AU Core and IPS
- the impact of using compliance declaration and imposed profiles
- whether the profiles are easy to interpret and apply
- whether FHIR tooling can process and enforce constraints
- ease of use for implementers, including whether a single implementation guide can provide all constraints reducing the need to reference AU Base, AU Core, and IPS separately

### FHIR mechanism for ensuring compliance
FHIR provides different key mechanisms for ensuring compliance across different specifications: 
1. [StructureDefinition Complies With Profile (compliesWith)](https://hl7.org/fhir/extensions/StructureDefinition-structuredefinition-compliesWithProfile.html)
   - Declares that a profile complies with a given standard (e.g., AU Core, IPS). 
   - A profile does not need to be directly derived from AU Core or IPS but needs to meet all their constraints.
   - Declaring compliesWith AU Core and IPS does not apply rules automatically. It states the intent to comply, meaning the profile itself needs to enforce the necessary constraints. 
1. [StructureDefinition Dependent Profiles (imposeProfile)](https://hl7.org/fhir/extensions/StructureDefinition-structuredefinition-imposeProfile.html)
   - Ensures conformance to another profile by applying constraints from the imposed profile.
   - More restrictive than [StructureDefinition Complies With Profile](https://hl7.org/fhir/extensions/StructureDefinition-structuredefinition-compliesWithProfile.html), as it inherits all rules of the imposed profile.
   - Ensures a profile applies all constraints from another profile without being directly derived.
1. [StructureDefinition Derived from Profiles (derive)](https://build.fhir.org/structuredefinition-definitions.html#StructureDefinition.baseDefinition)
   - Ensures conformance to another profile by applying constraints through inheritance from the base profile.
   - The resulting profiles is a 'profile' can apply further constraints to those inherited from the base profile.
   - A profile can only derive from one profile not multiple
   - Be wary of over-use - humans have trouble understanding the implications of deep stacks of constraining profiles. 

TBD

### Testing approach
To test the AU patient summary approach we have three main test scenarios that different profile approaches are tested against:

*	Test scenario 01: example FHIR resource compliant to AU Core and IPS
*	Test scenario 02: example FHIR resource compliant to AU Core but not IPS
*	Test scenario 03: example FHIR resource compliant to IPS but not AU Core

The total pool of examples includes:
*	set of 'control' examples (27 examples)
*	set of 'approach' examples, where there the 'control' set has been adjusted only to change the meta.profile value for each approach A to I.
  *	set of approach A examples (9 examples)
  * set of approach B examples (9 examples)
  * etc.

#### Set of control examples
Originating AU Core examples are the basis for the test examples: [MedicationRequest/paracetamol-codeine](https://build.fhir.org/ig/hl7au/au-fhir-core/MedicationRequest-paracetamol-codeine.html), [MedicationRequest/reaptan](https://build.fhir.org/ig/hl7au/au-fhir-core/MedicationRequest-reaptan.html) and [MedicationRequest/simvastatin](https://build.fhir.org/ig/hl7au/au-fhir-core/MedicationRequest-simvastatin.html). 

Those originating 3 examples become the set of 27 control examples by:
*	Three test scenario variants for each of the 3 original files (becomes 9 files)
*	For each of that set, there are then three controls on that using meta using meta.profile e.g. one control asserts only AU Core, another control asserts only IPS, the last control asserts both. 

In [qa.html](qa.html), if an instance passes, it does so whether `meta.profile` has one or multiple profiles. However, an error, warning, or information is triggered, qa.html does not display which profile is the reason. To make clear which profile assertion is triggering each error, warning, or information, we have taken the approach of 'variation' by meta.profile value.

<table border="1" cellspacing="0" cellpadding="0" width="100%">
    <thead>
  <tr>
    <th>Test scenario</th>
    <th>Example instance that is the control check against both AU Core & IPS using meta.profile</th>
    <th>Example instance that is the control check against AU Core only using meta.profile</th>
    <th>Example instance that is the control check against IPS only using meta.profile</th>
  </tr>
  </thead>
<tbody>
  <tr>
    <td rowspan="3">01 Example compliant to AU Core and IPS</td>
    <td>simvastin-01-aucoreips</td>
    <td>simvastin-01-aucore</td>
    <td>simvastin-01-ips</td>
  </tr>
  <tr>
    <td>reaptan-01-aucoreips</td>
    <td>reaptan-01-aucore</td>
    <td>reaptan-01-ips</td>
  </tr>
  <tr>
    <td>paracetamol-codeine-01-aucoreips</td>
    <td>paracetamol-codeine-01-aucore</td>
    <td>paracetamol-codeine-01-ips</td>
  </tr>
  <tr>
    <td rowspan="3">02 Example compliant to AU Core but not IPS</td>
    <td>simvastin-02-aucoreips</td>
    <td>simvastin-02-aucore</td>
    <td>simvastin-02-ips</td>
  </tr>
  <tr>
    <td>reaptan-02-aucoreips</td>
    <td>reaptan-02-aucore</td>
    <td>reaptan-02-ips</td>
  </tr>
  <tr>
    <td>paracetamol-codeine-02-aucoreips</td>
    <td>paracetamol-codeine-02-aucore</td>
    <td>paracetamol-codeine-02-ips</td>
  </tr>
  <tr>
    <td rowspan="3">03 Example compliant to IPS but not AU Core</td>
    <td>simvastin-03-aucoreips</td>
    <td>simvastin-03-aucore</td>
    <td>simvastin-03-ips</td>
  </tr>
  <tr>
    <td>reaptan-03-aucoreips</td>
    <td>reaptan-03-aucore</td>
    <td>reaptan-03-ips</td>
  </tr>
  <tr>
    <td>paracetamol-codeine-03-aucoreips</td>
    <td>paracetamol-codeine-03-aucore</td>
    <td>paracetamol-codeine-03-ips</td>
  </tr>
</tbody>
</table>


#### Set of approach examples
The table shows different profiling approaches for AU PS profiles. The table includes:
- Profile if and title- hyperlinked to the profile definition
- Profiling approach description - explains how the profile is derived and how it enforces AU Core and IPS constraints
- Test scenarios - indicates if the profile complies with AU Core, IPS, or both
- Example instances used for testing

Conventions used in testing: 
- Profile id, name, title and example instances include a suffix (e.g., -a) which identifies the specific profiling approach used
- Example instances used to test a profile carry the same suffix as the profile itself
  - The only variation between instances is in `meta.profile`, which declares compliance with the profile that has the same suffix.
- Each profile has a set of instances (01, 02, etc) to test different scenarios while maintaining the same profile suffix.
- Each example instance declares conformance to only one profile in `meta.profile` to make validation results clear. In [qa.html](qa.html), if an instance passes, it does so whether `meta.profile` has one or multiple profiles. However, if it fails, qa.html does not show which profile the instance failed against. To avoid this issue, we test one profile at a time.

<table border="1" cellspacing="0" cellpadding="0" width="100%">
    <thead>
  <tr>
    <th>Profile Title</th>
    <th>Profiling approach</th>
    <th>Test scenario</th>
    <th>Example id</th>
  </tr>
  </thead>
<tbody>
  <tr>
    <td rowspan="9"><a href="StructureDefinition-au-exp-ps-medicationrequest-a.html">EXPERIMENTAL AU PS MedicationRequest Approach A</a></td>
    <td rowspan="9">Approach A: <ul><li>Derive from AU Base</li><li>Apply AU Core</li><li>Apply IPS constraints</li><li>State compliesWith AU Core and IPS</li></ul> In this case the references to profiles are pointed to AU PS profiles where they exist.</td>
    <td rowspan="3">Validate examples compliant with IPS and AU Core</td>
    <td><a href="MedicationRequest-simvastatin-01-a.html">simvastatin-01-a</a></td>
  </tr>
  <tr>
    <td><a href="MedicationRequest-reaptan-01-a.html">reaptan-01-a</a></td>
  </tr>
  <tr>
    <td><a href="MedicationRequest-paracetamol-codeine-01-a.html">paracetamol-codeine-01-a</a></td>
  </tr>
  <tr>
    <td rowspan="3">Validate examples compliant with AU Core but not IPS</td>
    <td><a href="MedicationRequest-simvastatin-02-a.html">simvastatin-02-a</a></td>
  </tr>
  <tr>
    <td><a href="MedicationRequest-reaptan-02-a.html">reaptan-02-a</a></td>
  </tr>
  <tr>
    <td><a href="MedicationRequest-paracetamol-codeine-02-a.html">paracetamol-codeine-02-a</a></td>
  </tr>
  <tr>
    <td rowspan="3">Validate examples compliant with IPS but not AU Core</td>
    <td><a href="MedicationRequest-simvastatin-03-a.html">simvastatin-03-a</a></td>
  </tr>
  <tr>
    <td><a href="MedicationRequest-reaptan-03-a.html">reaptan-03-a</a></td>
  </tr>
  <tr>
    <td><a href="MedicationRequest-paracetamol-codeine-03-a.html">paracetamol-codeine-03-a</a></td>
  </tr>
  <tr>
    <td rowspan="9"><a href="StructureDefinition-au-exp-ps-medicationrequest-b.html">EXPERIMENTAL AU PS MedicationRequest Approach B</a></td>
    <td rowspan="9">Approach B: <ul><li>Derive from AU Core</li><li>Reapply AU Core constraints (so they appear in the diff similar to US Core approach)</li><li>Apply IPS constraints</li><li>State compliesWith IPS</li></ul> In this case the references to profiles are pointed to AU PS profiles where they exist.</td>
    <td rowspan="3">Validate examples compliant with IPS and AU Core</td>
    <td><a href="MedicationRequest-simvastatin-01-b.html">simvastatin-01-b</a></td>
  </tr>
  <tr>
    <td><a href="MedicationRequest-reaptan-01-b.html">reaptan-01-b</a></td>
  </tr>
  <tr>
    <td><a href="MedicationRequest-paracetamol-codeine-01-b.html">paracetamol-codeine-01-b</a></td>
  </tr>
  <tr>
    <td rowspan="3">Validate examples compliant with AU Core but not IPS</td>
    <td><a href="MedicationRequest-simvastatin-02-b.html">simvastatin-02-b</a></td>
  </tr>
  <tr>
    <td><a href="MedicationRequest-reaptan-02-b.html">reaptan-02-b</a></td>
  </tr>
  <tr>
    <td><a href="MedicationRequest-paracetamol-codeine-02-b.html">paracetamol-codeine-02-b</a></td>
  </tr>
  <tr>
    <td rowspan="3"><br>Validate examples compliant with IPS but not AU Core</td>
    <td><a href="MedicationRequest-simvastatin-03-b.html">simvastatin-03-b</a></td>
  </tr>
  <tr>
    <td><a href="MedicationRequest-reaptan-01-b.html">reaptan-01-b</a></td>
  </tr>
  <tr>
    <td><a href="MedicationRequest-paracetamol-codeine-03-b.html">paracetamol-codeine-03-b</a></td>
  </tr>
  <tr>
    <td rowspan="9"><a href="StructureDefinition-au-exp-ps-medicationrequest-c.html">EXPERIMENTAL AU PS MedicationRequest Approach C</a></td>
    <td rowspan="9">Approach C: <ul><li>Derive from AU Core</li><li>Apply IPS constraints</li><li>State compliesWith IPS</li></ul></td>
    <td rowspan="3">Validate examples compliant with IPS and AU Core</td>
    <td><a href="MedicationRequest-simvastatin-01-c.html">simvastatin-01-c</a></td>
  </tr>
  <tr>
    <td><a href="MedicationRequest-reaptan-01-c.html">reaptan-01-c</a></td>
  </tr>
  <tr>
    <td><a href="MedicationRequest-paracetamol-codeine-01-c.html">paracetamol-codeine-01-c</a></td>
  </tr>
  <tr>
    <td rowspan="3">Validate examples compliant with AU Core but not IPS</td>
    <td><a href="MedicationRequest-simvastatin-02-c.html">simvastatin-02-c</a></td>
  </tr>
  <tr>
    <td><a href="MedicationRequest-reaptan-02-c.html">reaptan-02-c</a></td>
  </tr>
  <tr>
    <td><a href="MedicationRequest-paracetamol-codeine-02-c.html">paracetamol-codeine-02-c</a></td>
  </tr>
  <tr>
    <td rowspan="3">Validate examples compliant with IPS but not AU Core</td>
    <td><a href="MedicationRequest-simvastatin-03-c.html">simvastatin-03-c</a></td>
  </tr>
  <tr>
    <td><a href="MedicationRequest-reaptan-03-c.html">reaptan-03-c</a></td>
  </tr>
  <tr>
    <td><a href="MedicationRequest-paracetamol-codeine-03-c.html">paracetamol-codeine-03-c</a></td>
  </tr>
  <tr>
    <td rowspan="9"><a href="StructureDefinition-au-exp-ps-medicationrequest-d.html">EXPERIMENTAL AU PS MedicationRequest Approach D</a></td>
    <td rowspan="9">Approach D: <ul><li>Derive from FHIR</li><li>Apply AU Base constraints (terminologies and identifiers)</li><li>Apply AU Core constraints</li><li>Apply IPS constraints</li><li>State compliesWith AU Core and IPS</li></ul></td>
    <td rowspan="3">Validate examples compliant with IPS and AU Core</td>
    <td><a href="MedicationRequest-simvastatin-01-d.html">simvastatin-01-d</a></td>
  </tr>
  <tr>
    <td><a href="MedicationRequest-reaptan-01-d.html">reaptan-01-d</a></td>
  </tr>
  <tr>
    <td><a href="MedicationRequest-paracetamol-codeine-01-d.html">paracetamol-codeine-01-d</a></td>
  </tr>
  <tr>
    <td rowspan="3">Validate examples compliant with AU Core but not IPS</td>
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
    <td rowspan="9"><a href="StructureDefinition-au-exp-ps-medicationrequest-e.html">EXPERIMENTAL AU PS MedicationRequest Approach E</a></td>
    <td rowspan="9">Approach E: <ul><li>Derived from IPS</li><li>Apply AU Core constraints where possible</li></ul> This is not complete yet as we would want to modify the references to IPS profiles to be AU PS profiles but IG Publisher builds fail if this is done.</td>
    <td rowspan="3">Validate examples compliant with IPS and AU Core</td>
    <td><a href="MedicationRequest-simvastatin-01-e.html">simvastatin-01-e</a></td>
  </tr>
  <tr>
    <td><a href="MedicationRequest-reaptan-01-e.html">reaptan-01-e</a></td>
  </tr>
  <tr>
    <td><a href="MedicationRequest-paracetamol-codeine-01-e.html">paracetamol-codeine-01-e</a></td>
  </tr>
  <tr>
    <td rowspan="3">Validate examples compliant with AU Core but not IPS</td>
    <td><a href="MedicationRequest-simvastatin-02-e.html">simvastatin-02-e</a></td>
  </tr>
  <tr>
    <td><a href="MedicationRequest-reaptan-02-e.html">reaptan-02-e</a></td>
  </tr>
  <tr>
    <td><a href="MedicationRequest-paracetamol-codeine-02-e.html">paracetamol-codeine-02-e</a></td>
  </tr>
  <tr>
    <td rowspan="3">Validate examples compliant with IPS but not AU Core</td>
    <td><a href="MedicationRequest-simvastatin-03-e.html">simvastatin-03-e</a></td>
  </tr>
  <tr>
    <td><a href="MedicationRequest-reaptan-03-e.html">reaptan-03-e</a></td>
  </tr>
  <tr>
    <td><a href="MedicationRequest-paracetamol-codeine-03-e.html">paracetamol-codeine-03-e</a></td>
  </tr>
  <tr>
    <td rowspan="9"><a href="StructureDefinition-au-exp-ps-medicationrequest-f.html">EXPERIMENTAL AU PS MedicationRequest Approach F</a></td>
    <td rowspan="9">Approach F: <ul><li>Derive from AU Core</li><li>use imposeProfile to enforce IPS constraints</li></ul></td>
    <td rowspan="3">Validate examples compliant with IPS and AU Core</td>
    <td><a href="MedicationRequest-simvastatin-01-f.html">simvastatin-01-f</a></td>
  </tr>
  <tr>
    <td><a href="MedicationRequest-reaptan-01-f.html">reaptan-01-f</a></td>
  </tr>
  <tr>
    <td><a href="MedicationRequest-paracetamol-codeine-01-f.html">paracetamol-codeine-01-f</a></td>
  </tr>
  <tr>
    <td rowspan="3"><br>Validate examples compliant with AU Core but not IPS</td>
    <td><a href="MedicationRequest-simvastatin-02-f.html">simvastatin-02-f</a></td>
  </tr>
  <tr>
    <td><a href="MedicationRequest-reaptan-02-f.html">reaptan-02-f</a></td>
  </tr>
  <tr>
    <td><a href="MedicationRequest-paracetamol-codeine-02-f.html">paracetamol-codeine-02-f</a></td>
  </tr>
  <tr>
    <td rowspan="3">Validate examples compliant with IPS but not AU Core</td>
    <td><a href="MedicationRequest-simvastatin-03-f.html">simvastatin-03-f</a></td>
  </tr>
  <tr>
    <td><a href="MedicationRequest-reaptan-03-f.html">reaptan-03-f</a></td>
  </tr>
  <tr>
    <td><a href="MedicationRequest-paracetamol-codeine-03-f.html">paracetamol-codeine-03-f</a></td>
  </tr>
  <tr>
    <td rowspan="9"><a href="StructureDefinition-au-exp-ps-medicationrequest-g.html">EXPERIMENTAL AU PS MedicationRequest Approach G</a> </td>
    <td rowspan="9">Approach G: <ul><li>Derive from IPS</li><li>use imposeProfile to enforce AU Core constraints</li></ul></td>
    <td rowspan="3">Validate examples compliant with IPS and AU Core</td>
    <td><a href="MedicationRequest-simvastatin-01-g.html">simvastatin-01-g</a></td>
  </tr>
  <tr>
    <td><a href="MedicationRequest-reaptan-01-g.html">reaptan-01-g</a></td>
  </tr>
  <tr>
    <td><a href="MedicationRequest-paracetamol-codeine-01-g.html">paracetamol-codeine-01-g</a></td>
  </tr>
  <tr>
    <td rowspan="3">Validate examples compliant with AU Core but not IPS</td>
    <td><a href="MedicationRequest-simvastatin-02-g.html">simvastatin-02-g</a></td>
  </tr>
  <tr>
    <td><a href="MedicationRequest-reaptan-02-g.html">reaptan-02-g</a></td>
  </tr>
  <tr>
    <td><a href="MedicationRequest-paracetamol-codeine-02-g.html">paracetamol-codeine-02-g</a></td>
  </tr>
  <tr>
    <td rowspan="3">Validate examples compliant with IPS but not AU Core</td>
    <td><a href="MedicationRequest-simvastatin-03-g.html">simvastatin-03-g</a></td>
  </tr>
  <tr>
    <td><a href="MedicationRequest-reaptan-03-g.html">reaptan-03-g</a></td>
  </tr>
  <tr>
    <td><a href="MedicationRequest-paracetamol-codeine-03-g.html">paracetamol-codeine-03-g</a></td>
  </tr>
  <tr>
    <td rowspan="9"><a href="StructureDefinition-au-exp-ps-medicationrequest-h.html">EXPERIMENTAL AU PS MedicationRequest Approach H</a></td>
    <td rowspan="9">Approach H: <ul><li>Derive from AU Base</li><li>use imposeProfile to enforce both AU Core and IPS constraints</li></ul></td>
    <td rowspan="3">Validate examples compliant with IPS and AU Core</td>
    <td><a href="MedicationRequest-simvastatin-01-h.html">simvastatin-01-h</a></td>
  </tr>
  <tr>
    <td><a href="MedicationRequest-reaptan-01-h.html">reaptan-01-h</a></td>
  </tr>
  <tr>
    <td><a href="MedicationRequest-paracetamol-codeine-01-h.html">paracetamol-codeine-01-h</a></td>
  </tr>
  <tr>
    <td rowspan="3">Validate examples compliant with AU Core but not IPS</td>
    <td><a href="MedicationRequest-simvastatin-02-h.html">simvastatin-02-h</a></td>
  </tr>
  <tr>
    <td><a href="MedicationRequest-reaptan-02-h.html">reaptan-02-h</a></td>
  </tr>
  <tr>
    <td><a href="MedicationRequest-paracetamol-codeine-02-h.html">paracetamol-codeine-02-h</a></td>
  </tr>
  <tr>
    <td rowspan="3">Validate examples compliant with IPS but not AU Core</td>
    <td><a href="MedicationRequest-simvastatin-03-h.html">simvastatin-03-h</a></td>
  </tr>
  <tr>
    <td><a href="MedicationRequest-reaptan-03-h.html">reaptan-03-h</a></td>
  </tr>
  <tr>
    <td><a href="MedicationRequest-paracetamol-codeine-03-h.html">paracetamol-codeine-03-h</a></td>
  </tr>
  <tr>
    <td rowspan="9"><a href="StructureDefinition-au-exp-ps-medicationrequest-i.html">EXPERIMENTAL AU PS MedicationRequest Approach I</a></td>
    <td rowspan="9">Approach I: <ul><li>Derive from the FHIR resource</li><li>State complies with AU Core and IPS</li></ul> Note this is a negative case. The profile does not include, directly or by reference, any rules to make the claims of assertion true. This profile demonstrates behaviour when a 'false' compliesWith is stated.</td>
    <td rowspan="3">Validate examples compliant with IPS and AU Core</td>
    <td><a href="MedicationRequest-simvastatin-01-i.html">simvastatin-01-i</a></td>
  </tr>
  <tr>
    <td><a href="MedicationRequest-reaptan-01-i.html">reaptan-01-i</a></td>
  </tr>
  <tr>
    <td><a href="MedicationRequest-paracetamol-codeine-01-i.html">paracetamol-codeine-01-i</a></td>
  </tr>
  <tr>
    <td rowspan="3">Validate examples compliant with AU Core but not IPS</td>
    <td><a href="MedicationRequest-simvastatin-02-i.html">simvastatin-02-i</a></td>
  </tr>
  <tr>
    <td><a href="MedicationRequest-reaptan-02-i.html">reaptan-02-i</a></td>
  </tr>
  <tr>
    <td><a href="MedicationRequest-paracetamol-codeine-02-i.html">paracetamol-codeine-02-i</a></td>
  </tr>
  <tr>
    <td rowspan="3">Validate examples compliant with IPS but not AU Core</td>
    <td><a href="MedicationRequest-simvastatin-03-i.html">simvastatin-03-i</a></td>
  </tr>
  <tr>
    <td><a href="MedicationRequest-reaptan-03-i.html">reaptan-03-i</a></td>
  </tr>
  <tr>
    <td><a href="MedicationRequest-paracetamol-codeine-03-i.html">paracetamol-codeine-03-i</a></td>
  </tr>
</tbody>
</table>


