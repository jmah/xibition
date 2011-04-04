# xibition &mdash; Readable diffs for XIB files
## About

xibition is a lossy, human-readable converter for Xcode / Interface Builder XIB (and NIB) files. It is designed for use with git's textconv feature to provide nicer diffs of interface files.

xibition doesn't aim to present every attribute of every object. Instead, it aims for output that's easily skimmed visually.

Output is generated for the following object properties: names, labels, titles, frames (positions), tool tips, actions, outlets, and bindings. All other properties (font and text settings, etc.) are condensed into a SHA1 hash (`attributes_sha1`) to show when they change, but without an additional 10–30 lines per object.


## Installation

xibition depends on the Ruby Gems [escape](http://rubygems.org/gems/escape) and [plist](http://rubygems.org/gems/plist). (I'd really like be independent of `escape`, if someone is able to contribute.)

Add the following to your gitconfig (I recommend the global one, `~/.gitconfig`):

<pre>
[diff "xibition"]
    textconv = /path/to/xibition
    cachetextconv = true
    xfuncname = "^(@interface[ \t].*)$\n^[0-9 |-]*----\\+ (.+)$"
</pre>

Then, in a [gitattributes][] file (`.git/info/attributes`, `/etc/gitattributes`, or `.gitattributes`):

<pre>
*.xib   diff=xibition
</pre>

[gitattributes]: http://www.kernel.org/pub/software/scm/git/docs/gitattributes.html

While you're there, add one for Objective-C context headers:

<pre>
*.m     diff=objc
</pre>


## Example Output

### With xibition

<pre>
<span style="font-weight: bold; color: black">diff --git a/en.lproj/PBASessionDocument.xib b/en.lproj/PBASessionDocument.xib</span>
<span style="font-weight: bold; color: black">index 341e562..8932bd3 100644</span>
<span style="font-weight: bold; color: black">--- a/en.lproj/PBASessionDocument.xib</span>
<span style="font-weight: bold; color: black">+++ b/en.lproj/PBASessionDocument.xib</span>
<span style="color: purple">@@ -82,10 +82,17 @@ Text Field Cell - Text Cell</span>
    5|   6|100304|100303|100294|100298|100299|100302| Attributes: a0119822941fea83fa27a7f77724c621feab09fa
    5|   6|100304|100303|100294|100298|
    5|   6|100304|100303|100294|100298|------+ Table Column - Value
<span style="color: red">-   5|   6|100304|100303|100294|100298|100300| Attributes: d1db9fd61d74733954093554cc0475bd8ca5ccb2</span>
<span style="color: green">+   5|   6|100304|100303|100294|100298|100300| Attributes: 6fcea276383d412fd2234242de57311706f5922a</span>
    5|   6|100304|100303|100294|100298|100300|
    5|   6|100304|100303|100294|100298|100300| value: (100305| Message Headers - Dictionary Controller).arrangedObjects.value
    5|   6|100304|100303|100294|100298|100300|
<span style="color: green">+   5|   6|100304|100303|100294|100298|100300| editable: (100318| Messages - Tree Controller).selection</span>
<span style="color: green">+   5|   6|100304|100303|100294|100298|100300|     NSMultipleValuesPlaceholder = 0</span>
<span style="color: green">+   5|   6|100304|100303|100294|100298|100300|     NSNoSelectionPlaceholder = 0</span>
<span style="color: green">+   5|   6|100304|100303|100294|100298|100300|     NSNotApplicablePlaceholder = 0</span>
<span style="color: green">+   5|   6|100304|100303|100294|100298|100300|     NSNullPlaceholder = 0</span>
<span style="color: green">+   5|   6|100304|100303|100294|100298|100300|     NSValueTransformerName = PBAMessageIsRequest</span>
<span style="color: green">+   5|   6|100304|100303|100294|100298|100300|</span>
    5|   6|100304|100303|100294|100298|100300|------+ Text Field Cell - Text Cell
    5|   6|100304|100303|100294|100298|100300|100301| Attributes: a0119822941fea83fa27a7f77724c621feab09fa
    5|   6|100304|100303|
<span style="color: purple">@@ -445,10 +452,9 @@ Messages - Tree Controller</span>
 100318| contentArray: (-2| File's Owner).messages
 
 ------+ Message Headers - Dictionary Controller
<span style="color: red">-100305| Attributes: 119fba4fb04d11d0ced6946471ac31fe5acf24ec</span>
<span style="color: green">+100305| Attributes: e4572161b7dc83b4b720b9d211afc2d9b2b309e8</span>
 100305|
 100305| contentDictionary: (100318| Messages - Tree Controller).selection.allHTTPHeaderFields
<span style="color: red">-100305|     NSConditionallySetsEditable = false</span>
 
 ------+ Interpreted Fields - Tree Controller
 100053| Attributes: 8ebce06afa6947ce6516527a699c01f51b683a71
</pre>


### Without xibition

This is the same commit as above. You can disable conversion by passing `--no-textconv` to `git diff` (or `git log`, etc.).

<pre>
<span style="font-weight: bold; color: black">diff --git a/en.lproj/PBASessionDocument.xib b/en.lproj/PBASessionDocument.xib</span>
<span style="font-weight: bold; color: black">index 341e562..8932bd3 100644</span>
<span style="font-weight: bold; color: black">--- a/en.lproj/PBASessionDocument.xib</span>
<span style="font-weight: bold; color: black">+++ b/en.lproj/PBASessionDocument.xib</span>
<span style="color: purple">@@ -2,13 +2,13 @@</span>
 &lt;archive type="com.apple.InterfaceBuilder3.Cocoa.XIB" version="8.00"&gt;
 	&lt;data&gt;
 		&lt;int key="IBDocument.SystemTarget"&gt;1070&lt;/int&gt;
<span style="color: red">-		&lt;string key="IBDocument.SystemVersion"&gt;11A390&lt;/string&gt;</span>
<span style="color: red">-		&lt;string key="IBDocument.InterfaceBuilderVersion"&gt;1510&lt;/string&gt;</span>
<span style="color: red">-		&lt;string key="IBDocument.AppKitVersion"&gt;1110.91&lt;/string&gt;</span>
<span style="color: red">-		&lt;string key="IBDocument.HIToolboxVersion"&gt;544.00&lt;/string&gt;</span>
<span style="color: green">+		&lt;string key="IBDocument.SystemVersion"&gt;11A419&lt;/string&gt;</span>
<span style="color: green">+		&lt;string key="IBDocument.InterfaceBuilderVersion"&gt;1530&lt;/string&gt;</span>
<span style="color: green">+		&lt;string key="IBDocument.AppKitVersion"&gt;1115.2&lt;/string&gt;</span>
<span style="color: green">+		&lt;string key="IBDocument.HIToolboxVersion"&gt;549.00&lt;/string&gt;</span>
 		&lt;object class="NSMutableDictionary" key="IBDocument.PluginVersions"&gt;
 			&lt;string key="NS.key.0"&gt;com.apple.InterfaceBuilder.CocoaPlugin&lt;/string&gt;
<span style="color: red">-			&lt;string key="NS.object.0"&gt;1510&lt;/string&gt;</span>
<span style="color: green">+			&lt;string key="NS.object.0"&gt;1530&lt;/string&gt;</span>
 		&lt;/object&gt;
 		&lt;array key="IBDocument.IntegratedClassDependencies"&gt;
 			&lt;string&gt;NSMatrix&lt;/string&gt;
<span style="color: purple">@@ -316,6 +316,7 @@</span>
 																	&lt;/object&gt;
 																	&lt;int key="NSResizingMask"&gt;3&lt;/int&gt;
 																	&lt;bool key="NSIsResizeable"&gt;YES&lt;/bool&gt;
<span style="color: green">+																	&lt;bool key="NSIsEditable"&gt;YES&lt;/bool&gt;</span>
 																	&lt;reference key="NSTableView" ref="666981949"/&gt;
 																&lt;/object&gt;
 															&lt;/array&gt;
<span style="color: purple">@@ -414,16 +415,13 @@</span>
 																&lt;string key="NSFrameSize"&gt;{505, 17}&lt;/string&gt;
 																&lt;reference key="NSSuperview" ref="677073926"/&gt;
 																&lt;reference key="NSWindow"/&gt;
<span style="color: red">-																&lt;reference key="NSNextKeyView" ref="221544145"/&gt;</span>
<span style="color: green">+																&lt;reference key="NSNextKeyView" ref="150240800"/&gt;</span>
 																&lt;reference key="NSTableView" ref="626158917"/&gt;
 															&lt;/object&gt;
<span style="color: red">-															&lt;object class="_NSCornerView" key="NSCornerView" id="221544145"&gt;</span>
<span style="color: red">-																&lt;reference key="NSNextResponder" ref="563558642"/&gt;</span>
<span style="color: green">+															&lt;object class="_NSCornerView" key="NSCornerView"&gt;</span>
<span style="color: green">+																&lt;nil key="NSNextResponder"/&gt;</span>
 																&lt;int key="NSvFlags"&gt;-2147483392&lt;/int&gt;
 																&lt;string key="NSFrame"&gt;{{411, 0}, {16, 17}}&lt;/string&gt;
<span style="color: red">-																&lt;reference key="NSSuperview" ref="563558642"/&gt;</span>
<span style="color: red">-																&lt;reference key="NSWindow"/&gt;</span>
<span style="color: red">-																&lt;reference key="NSNextKeyView" ref="150240800"/&gt;</span>
 															&lt;/object&gt;
 															&lt;array class="NSMutableArray" key="NSTableColumns"&gt;
 																&lt;object class="NSTableColumn" id="77071068"&gt;
<span style="color: purple">@@ -586,7 +584,6 @@</span>
 													&lt;reference key="NSBGColor" ref="847445910"/&gt;
 													&lt;int key="NScvFlags"&gt;4&lt;/int&gt;
 												&lt;/object&gt;
<span style="color: red">-												&lt;reference ref="221544145"/&gt;</span>
 											&lt;/array&gt;
 											&lt;string key="NSFrameSize"&gt;{505, 219}&lt;/string&gt;
 											&lt;reference key="NSSuperview" ref="438416439"/&gt;
<span style="color: purple">@@ -597,7 +594,6 @@</span>
 											&lt;reference key="NSHScroller" ref="763140228"/&gt;
 											&lt;reference key="NSContentView" ref="150240800"/&gt;
 											&lt;reference key="NSHeaderClipView" ref="677073926"/&gt;
<span style="color: red">-											&lt;reference key="NSCornerView" ref="221544145"/&gt;</span>
 											&lt;bytes key="NSScrollAmts"&gt;QSAAAEEgAABBmAAAQZgAAA&lt;/bytes&gt;
 										&lt;/object&gt;
 										&lt;object class="NSCustomView" id="309295333"&gt;
<span style="color: purple">@@ -1617,6 +1613,7 @@</span>
 				&lt;string key="NSTreeContentChildrenKey"&gt;responseMessages&lt;/string&gt;
 			&lt;/object&gt;
 			&lt;object class="NSDictionaryController" id="735985140"&gt;
<span style="color: green">+				&lt;bool key="NSEditable"&gt;YES&lt;/bool&gt;</span>
 				&lt;bool key="NSPreservesSelection"&gt;YES&lt;/bool&gt;
 				&lt;bool key="NSFilterRestrictsInsertion"&gt;YES&lt;/bool&gt;
 				&lt;array key="NSSortDescriptors"&gt;
<span style="color: purple">@@ -2039,22 +2036,6 @@</span>
 				&lt;/object&gt;
 				&lt;object class="IBConnectionRecord"&gt;
 					&lt;object class="IBBindingConnection" key="connection"&gt;
<span style="color: red">-						&lt;string key="label"&gt;value: arrangedObjects.key&lt;/string&gt;</span>
<span style="color: red">-						&lt;reference key="source" ref="908225424"/&gt;</span>
<span style="color: red">-						&lt;reference key="destination" ref="735985140"/&gt;</span>
<span style="color: red">-						&lt;object class="NSNibBindingConnector" key="connector"&gt;</span>
<span style="color: red">-							&lt;reference key="NSSource" ref="908225424"/&gt;</span>
<span style="color: red">-							&lt;reference key="NSDestination" ref="735985140"/&gt;</span>
<span style="color: red">-							&lt;string key="NSLabel"&gt;value: arrangedObjects.key&lt;/string&gt;</span>
<span style="color: red">-							&lt;string key="NSBinding"&gt;value&lt;/string&gt;</span>
<span style="color: red">-							&lt;string key="NSKeyPath"&gt;arrangedObjects.key&lt;/string&gt;</span>
<span style="color: red">-							&lt;int key="NSNibBindingConnectorVersion"&gt;2&lt;/int&gt;</span>
<span style="color: red">-						&lt;/object&gt;</span>
<span style="color: red">-					&lt;/object&gt;</span>
<span style="color: red">-					&lt;int key="connectionID"&gt;100308&lt;/int&gt;</span>
<span style="color: red">-				&lt;/object&gt;</span>
<span style="color: red">-				&lt;object class="IBConnectionRecord"&gt;</span>
<span style="color: red">-					&lt;object class="IBBindingConnection" key="connection"&gt;</span>
 						&lt;string key="label"&gt;value: arrangedObjects.value&lt;/string&gt;
 						&lt;reference key="source" ref="249399347"/&gt;
 						&lt;reference key="destination" ref="735985140"/&gt;
<span style="color: purple">@@ -2155,26 +2136,6 @@</span>
 					&lt;int key="connectionID"&gt;100324&lt;/int&gt;
 				&lt;/object&gt;
 				&lt;object class="IBConnectionRecord"&gt;
<span style="color: red">-					&lt;object class="IBBindingConnection" key="connection"&gt;</span>
<span style="color: red">-						&lt;string key="label"&gt;contentDictionary: selection.allHTTPHeaderFields&lt;/string&gt;</span>
<span style="color: red">-						&lt;reference key="source" ref="735985140"/&gt;</span>
<span style="color: red">-						&lt;reference key="destination" ref="6038189"/&gt;</span>
<span style="color: red">-						&lt;object class="NSNibBindingConnector" key="connector"&gt;</span>
<span style="color: red">-							&lt;reference key="NSSource" ref="735985140"/&gt;</span>
<span style="color: red">-							&lt;reference key="NSDestination" ref="6038189"/&gt;</span>
<span style="color: red">-							&lt;string key="NSLabel"&gt;contentDictionary: selection.allHTTPHeaderFields&lt;/string&gt;</span>
<span style="color: red">-							&lt;string key="NSBinding"&gt;contentDictionary&lt;/string&gt;</span>
<span style="color: red">-							&lt;string key="NSKeyPath"&gt;selection.allHTTPHeaderFields&lt;/string&gt;</span>
<span style="color: red">-							&lt;object class="NSDictionary" key="NSOptions"&gt;</span>
<span style="color: red">-								&lt;string key="NS.key.0"&gt;NSConditionallySetsEditable&lt;/string&gt;</span>
<span style="color: red">-								&lt;boolean value="NO" key="NS.object.0"/&gt;</span>
<span style="color: red">-							&lt;/object&gt;</span>
<span style="color: red">-							&lt;int key="NSNibBindingConnectorVersion"&gt;2&lt;/int&gt;</span>
<span style="color: red">-						&lt;/object&gt;</span>
<span style="color: red">-					&lt;/object&gt;</span>
<span style="color: red">-					&lt;int key="connectionID"&gt;100328&lt;/int&gt;</span>
<span style="color: red">-				&lt;/object&gt;</span>
<span style="color: red">-				&lt;object class="IBConnectionRecord"&gt;</span>
 					&lt;object class="IBOutletConnection" key="connection"&gt;
 						&lt;string key="label"&gt;messagesOutlineView&lt;/string&gt;
 						&lt;reference key="source" ref="512844837"/&gt;
<span style="color: purple">@@ -2451,6 +2412,61 @@</span>
 					&lt;/object&gt;
 					&lt;int key="connectionID"&gt;100416&lt;/int&gt;
 				&lt;/object&gt;
<span style="color: green">+				&lt;object class="IBConnectionRecord"&gt;</span>
<span style="color: green">+					&lt;object class="IBBindingConnection" key="connection"&gt;</span>
<span style="color: green">+						&lt;string key="label"&gt;contentDictionary: selection.allHTTPHeaderFields&lt;/string&gt;</span>
<span style="color: green">+						&lt;reference key="source" ref="735985140"/&gt;</span>
<span style="color: green">+						&lt;reference key="destination" ref="6038189"/&gt;</span>
<span style="color: green">+						&lt;object class="NSNibBindingConnector" key="connector"&gt;</span>
<span style="color: green">+							&lt;reference key="NSSource" ref="735985140"/&gt;</span>
<span style="color: green">+							&lt;reference key="NSDestination" ref="6038189"/&gt;</span>
<span style="color: green">+							&lt;string key="NSLabel"&gt;contentDictionary: selection.allHTTPHeaderFields&lt;/string&gt;</span>
<span style="color: green">+							&lt;string key="NSBinding"&gt;contentDictionary&lt;/string&gt;</span>
<span style="color: green">+							&lt;string key="NSKeyPath"&gt;selection.allHTTPHeaderFields&lt;/string&gt;</span>
<span style="color: green">+							&lt;int key="NSNibBindingConnectorVersion"&gt;2&lt;/int&gt;</span>
<span style="color: green">+						&lt;/object&gt;</span>
<span style="color: green">+					&lt;/object&gt;</span>
<span style="color: green">+					&lt;int key="connectionID"&gt;100417&lt;/int&gt;</span>
<span style="color: green">+				&lt;/object&gt;</span>
<span style="color: green">+				&lt;object class="IBConnectionRecord"&gt;</span>
<span style="color: green">+					&lt;object class="IBBindingConnection" key="connection"&gt;</span>
<span style="color: green">+						&lt;string key="label"&gt;value: arrangedObjects.key&lt;/string&gt;</span>
<span style="color: green">+						&lt;reference key="source" ref="908225424"/&gt;</span>
<span style="color: green">+						&lt;reference key="destination" ref="735985140"/&gt;</span>
<span style="color: green">+						&lt;object class="NSNibBindingConnector" key="connector"&gt;</span>
<span style="color: green">+							&lt;reference key="NSSource" ref="908225424"/&gt;</span>
<span style="color: green">+							&lt;reference key="NSDestination" ref="735985140"/&gt;</span>
<span style="color: green">+							&lt;string key="NSLabel"&gt;value: arrangedObjects.key&lt;/string&gt;</span>
<span style="color: green">+							&lt;string key="NSBinding"&gt;value&lt;/string&gt;</span>
<span style="color: green">+							&lt;string key="NSKeyPath"&gt;arrangedObjects.key&lt;/string&gt;</span>
<span style="color: green">+							&lt;int key="NSNibBindingConnectorVersion"&gt;2&lt;/int&gt;</span>
<span style="color: green">+						&lt;/object&gt;</span>
<span style="color: green">+					&lt;/object&gt;</span>
<span style="color: green">+					&lt;int key="connectionID"&gt;100419&lt;/int&gt;</span>
<span style="color: green">+				&lt;/object&gt;</span>
<span style="color: green">+				&lt;object class="IBConnectionRecord"&gt;</span>
<span style="color: green">+					&lt;object class="IBBindingConnection" key="connection"&gt;</span>
<span style="color: green">+						&lt;string key="label"&gt;editable: selection&lt;/string&gt;</span>
<span style="color: green">+						&lt;reference key="source" ref="249399347"/&gt;</span>
<span style="color: green">+						&lt;reference key="destination" ref="6038189"/&gt;</span>
<span style="color: green">+						&lt;object class="NSNibBindingConnector" key="connector"&gt;</span>
<span style="color: green">+							&lt;reference key="NSSource" ref="249399347"/&gt;</span>
<span style="color: green">+							&lt;reference key="NSDestination" ref="6038189"/&gt;</span>
<span style="color: green">+							&lt;string key="NSLabel"&gt;editable: selection&lt;/string&gt;</span>
<span style="color: green">+							&lt;string key="NSBinding"&gt;editable&lt;/string&gt;</span>
<span style="color: green">+							&lt;string key="NSKeyPath"&gt;selection&lt;/string&gt;</span>
<span style="color: green">+							&lt;dictionary key="NSOptions"&gt;</span>
<span style="color: green">+								&lt;integer value="0" key="NSMultipleValuesPlaceholder"/&gt;</span>
<span style="color: green">+								&lt;integer value="0" key="NSNoSelectionPlaceholder"/&gt;</span>
<span style="color: green">+								&lt;integer value="0" key="NSNotApplicablePlaceholder"/&gt;</span>
<span style="color: green">+								&lt;integer value="0" key="NSNullPlaceholder"/&gt;</span>
<span style="color: green">+								&lt;string key="NSValueTransformerName"&gt;PBAMessageIsRequest&lt;/string&gt;</span>
<span style="color: green">+							&lt;/dictionary&gt;</span>
<span style="color: green">+							&lt;int key="NSNibBindingConnectorVersion"&gt;2&lt;/int&gt;</span>
<span style="color: green">+						&lt;/object&gt;</span>
<span style="color: green">+					&lt;/object&gt;</span>
<span style="color: green">+					&lt;int key="connectionID"&gt;100425&lt;/int&gt;</span>
<span style="color: green">+				&lt;/object&gt;</span>
 			&lt;/array&gt;
 			&lt;object class="IBMutableOrderedSet" key="objectRecords"&gt;
 				&lt;array key="orderedObjects"&gt;
<span style="color: purple">@@ -3362,7 +3378,7 @@</span>
 			&lt;nil key="activeLocalization"/&gt;
 			&lt;dictionary class="NSMutableDictionary" key="localizations"/&gt;
 			&lt;nil key="sourceID"/&gt;
<span style="color: red">-			&lt;int key="maxID"&gt;100416&lt;/int&gt;</span>
<span style="color: green">+			&lt;int key="maxID"&gt;100425&lt;/int&gt;</span>
 		&lt;/object&gt;
 		&lt;object class="IBClassDescriber" key="IBDocument.Classes"&gt;
 			&lt;array class="NSMutableArray" key="referencedPartialClassDescriptions"&gt;
</pre>


## License

xibition is under an MIT license. The full text is in the `LICENSE.txt` file.
