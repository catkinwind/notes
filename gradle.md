# Problems met

1. Dependencies downloaded but not imported.

  this is about apache's commons-lang3 artifact. I was using "org.apache.commons:commons-lang3:3.6", the jar file is downloaded, but not imported the the build path. Then I changed it to :
  group: "org.apache.commons", name: "commons-lang", version: "3.6"

  Bingo, it worked. I think the parser for the previous string may be treatingcommons-lang3 improperly.
