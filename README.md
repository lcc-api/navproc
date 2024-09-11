# NavProc Corpus
These dataset represent the efforts of Language Computer Corp. under the ONR Science of AI Program 2020-2023.

These are released under the CreativeCommons-Attribution-NonCommercial-ShareAlike v4.0 license (https://creativecommons.org/licenses/by-nc-sa/4.0/legalcode).

(Mohler et al. 2024 -- Introducing LCC's NavProc 1.0 Corpus: Annotated Procedural Texts in the Naval Domain)

---
Data is split across several files contained in files (one per procedural unit) in the following directories:

label -- contains the heading associated with each procedural unit
raw -- contains the raw characters associated with each procedural unit (after curation to remove PDF artifacts)
tok -- tsv-style file contianing the curated token content for a procedural unit with information linking back to the raw character offsets in the raw file
sent -- tsv-style file containing the curated sentence content for a procedural unit with information linking back to the tokens for that procedural unit
zone -- tsv-style file containing the curated zoning content for a procedural unit with information linking back kto the sentences for that procedural unit
trig -- tsv-style file continaing all frame and semantic link triggers annotated for a given procedural unit with information linking to the tokens within the procedural unit
roles -- tsv-style file containing all roles annotated for a given procedural unit with information linking to its trigger and the tokens within the procedural unit

Note that sub-procedural units (e.g., 7.1.1 within 7.1) are provided as standalone files within the "inner" directory for each of the above directories.

String representations of sentences, tokens, annotations, etc. (outside of the raw file) are provided for convenience only and are not intended to be parsed directly.

---
json -- contains an omnibus representation merging all of the above standalone files into a single structure defined by the following Java(+Lombok) structure:

  @Data
  private static class AnnotatedDocument {
    @Data
    private static class ADToken {
      final int offset;
      final int startChar;
      final int charLength;
      final String tokenTag;
      final String secondaryTokenTag;
      final String normalized;
    }

    @Data
    private static class ADSent {
      final int offset;
      final int startToken;
      final int tokenLength;
      final String normalized;
    }

    @Data
    private static class ADZone {
      final int startSentence;
      final int lastSentence;
      final String type;
      final String normalized;
    }

    @Data
    private static class ADFrame {
      final String id;
      final String triggerType;
      final int triggerStartToken;
      final int triggerTokenLength;

      final int headStartToken;
      final int headTokenLength;
      final String normalizedTrigger;
      final String normalizedHead;
      final String triggerInContext;
      List<ADRole> roles = Lists.newArrayList();
    }

    @Data
    private static class ADRole {
      final String roleID;
      final String triggerID;
      final String roleType;

      final int roleStartToken;
      final int roleTokenLength;

      final int roleHeadStartToken;
      final int roleHeadTokenLength;
      final String normalizedRole;
      final String normalizedRoleHead;
      final String normalizedTrigger;
      final String roleInContext;
    }

    private final String raw;
    private final String label;
    private final List<ADToken> tokens;
    private final List<ADSent> sentences;
    private final List<ADZone> zones;
    private final List<ADFrame> frames;
  }



