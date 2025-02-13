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
Originating AU Core examples simvastin, reaptan, and paracetamol-codeine are the basis for the test examples. There are:
*	Three base originating files: simvastin, repeatan, paracetamol-codeine
*	Three test scenario variants for each of the files testing 01 Compliant to AU Core & IPS, 02 Compliant to AU Core but not IPS, 03 Compliant to IPS but not AU Core
*	There are three controls to test validation behaviour for each of the test scenario variants using meta.profile e.g. one control asserts only AU Core, another control asserts only IPS, the last control asserts both

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
    <th>Title</th>
    <th>Profiling approach</th>
    <th>Test scenario</th>
    <th>Example id</th>
  </tr>
  </thead>
<tbody>
  <tr>
    <td rowspan="9"><a href="StructureDefinition-au-ps-medicationrequest-a.html">AU PS MedicationRequest A</a></td>
    <td rowspan="9">Derive from AU Base, apply AU Core and IPS constraints, and state compliesWith AU Core and IPS. In this case the references to profiles are pointed to AU PS profiles where they exist.</td>
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
    <td rowspan="9"><a href="StructureDefinition-au-ps-medicationrequest-b.html">AU PS MedicationRequest B</a></td>
    <td rowspan="9">Derive from AU Core, reapply AU Core constraints, apply IPS constraints, and state compliesWith IPS. In this case the references to profiles are pointed to AU PS profiles where they exist.</td>
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
    <td rowspan="9"><a href="StructureDefinition-au-ps-medicationrequest-c.html">AU PS MedicationRequest C</a></td>
    <td rowspan="9">Derive from AU Core, apply IPS constraints, and state compliesWith IPS. In this </td>
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
    <td rowspan="9"><a href="StructureDefinition-au-ps-medicationrequest-d.html">AU PS MedicationRequest D</a></td>
    <td rowspan="9">Derive from FHIR, apply AU Base terminologies and identifiers, then apply AU Core and IPS constraints, and state compliesWith AU Core and IPS.</td>
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
    <td rowspan="9"><a href="StructureDefinition-au-ps-medicationrequest-e.html">AU PS MedicationRequest E</a></td>
    <td rowspan="9">Derived from IPS and constrained with AU Core rules where possible. This is not complete yet as we would want to modify the references to IPS profiles to be AU PS profiles but that is currently failing in IG Publisher tooling.</td>
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
    <td rowspan="9"><a href="StructureDefinition-au-ps-medicationrequest-f.html">AU PS MedicationRequest F</a></td>
    <td rowspan="9">Derive from AU Core and use imposeProfile to enforce IPS constraints.</td>
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
    <td rowspan="9"><a href="StructureDefinition-au-ps-medicationrequest-g.html">AU PS MedicationRequest G</a> </td>
    <td rowspan="9">Derive from IPS and use imposeProfile to enforce AU Core constraints.</td>
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
    <td rowspan="9"><a href="StructureDefinition-au-ps-medicationrequest-h.html">AU PS MedicationRequest H</a></td>
    <td rowspan="9">Derive from AU Base and use imposeProfile to enforce both AU Core and IPS constraints.</td>
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
    <td rowspan="9"><a href="StructureDefinition-au-ps-medicationrequest-i.html">AU PS MedicationRequest I</a></td>
    <td rowspan="9">Derive from the FHIR resource, and states complies with AU Core and IPS but does not include directly in the profile any rules to make this true - this is a negative case, that shows behaviour when a 'false' complies with is stated.</td>
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


