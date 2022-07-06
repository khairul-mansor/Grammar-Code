# Grammar-Code

## Details of the project

In this project, I created grammar codes in GRXML file format for English and Malay language. The goal is to create grammar codes that would parse the user expressions for use in Speech Recognition technology.

I created grammar codes for 21 components:

	expire_date
	calendar_range
	calendar
	calendar_commons
	anaphora
	duration range
	duration
	temperature
	units
	distance
	amount
	synonyms
	global
	generic_order
	quantity_rel
	double
	cardinal number
	ordinal number
	boolean

### Brief introduction into grxml grammars

Grammars are collections of rules that define what are the valid elements (vocabulary words or phrases) and how they can be combined. The grammars that we are developing and localizing are designed for parsing. During parsing the input is matched to the grammar and if the match is found, the formalized meaning of the input phrase is returned.

For example, consider a grammar for Boolean in English. If we start from scratch, the first rule we will write will cover only “yes” and “no”:


		<rule id="Boolean" scope="public">
		<one-of>
			<item> yes <tag>BOOLEAN=”true”</tag></item> 
			<item> no <tag>BOOLEAN=”false”</tag></item>
		</one-of>
		</rule>

Where there are the following basic elements:


	<item> corresponds to a smallest grammar element (individual word or a phrases that has to be taken as whole)
	<tag> contain the meaning that is assigned to the item
	<one-of> indicates that the grammar has to choose one of the items on the list.


This rule will be able to parse only “yes” or “no”.

Next step is to add other ways of saying yes and no in English:


	<rule id="Boolean" scope="public">
	<one-of>
		<item> 
			<one-of>
				<item>yes</item>
				<item>yup</item>
				<item>yeah</item>
			</one-of>
		<tag>BOOLEAN=”true”</tag></item> 
		<item>
			<one-of>
				<item>no</item>
				<item>nope</item>
			</one-of>
		<tag>BOOLEAN=”false”</tag></item>
	</one-of>
	</rule>

Now the two original items include several options each and the grammar will be able to parse not only “yes” and “no”, but also “yeah”, “nope”, etc. and assign the correct meaning to them.

The rule can also contain more complex expressions that will combine different items. For example, if the grammar has to cover also the expressions like “it is true”, “it’s correct”, “it is correct”, “it’s true”, new combinations of items have to be added to the rule:


	<rule id="Boolean" scope="public">
	<one-of>
		<item> 
			<one-of>
				<item>yes</item>
				<item>yup</item>
				<item>yeah</item>

				<item>
					<item>
						<one-of>
							<item>it is</item>
							<item>it’s</item>
						</one-of>
					</item>
					<item>
						<one-of>
							<item> true </item>
							<item>correct</item>
						</one-of>
					</item>
				</item>

			</one-of>
		<tag>BOOLEAN=”true”</tag></item> 
		<item>
			<one-of>
				<item>no</item>
				<item>nope</item>
			</one-of>
		<tag>BOOLEAN=”false”</tag></item>
	</one-of>
	</rule>

The new item is a sequence of two items. 

In order to indicate that “it is” and “it’s” are optional, it is possible to indicate that the item that contains them can be repeated zero or one time:

	<item repeat=”0-1”>
		<one-of>
			<item>it is</item>
			<item>it’s</item>
		</one-of>
	</item>

Most grammars contain more than one rule and these rules can reference each other. For example, Boolean expressions can be combined with adverbs indicating the strength of agreement and disagreement. These cases will be covered by adding a rule for such adverbs and then reference it in the main rule:

	New rule:
	<rule id="Adverb_Entirely ">
		<one-of>
			<item>really</item>
			<item>perfectly</item>
			<item>absolutely</item>
			<item>totally</item>
			<item>all</item>
			<item>entirely</item>
			<item>completely</item>
			<item>fully</item>
			<item>exactly</item>
		</one-of>
	</rule>

Referencing it in another rule:

	<item>
		<item>
			<one-of>
				<item>it is</item>
				<item>it’s</item>
			</one-of>
		</item>
		<item repeat="0-1">
			<ruleref uri="#Adverb_Entirely"/>
		</item>
		<item>
			<one-of>
				<item> true </item>
				<item>correct</item>
			</one-of>
		</item>
	</item>


#### The complete grammar (with header):

	<?xml version='1.0' encoding='ISO-8859-1'?>
	<grammar xml:lang="en-US" version="1.0" root="Boolean" xmlns="http://www.w3.org/2001/06/grammar">

	<rule id="Boolean" scope="public">
	<one-of>
		<item> 
			<one-of>
				<item>yes</item>
				<item>yup</item>
				<item>yeah</item>

				<item>
					<item>
						<one-of>
							<item>it is</item>
							<item>it’s</item>
						</one-of>
					</item>
					<item repeat="0-1">
						<ruleref uri="#Adverb_Entirely"/>
					</item>
					<item>
						<one-of>
							<item> true </item>
							<item>correct</item>
						</one-of>
					</item>
				</item>
				<item>
					<one-of>
						<item> true </item>
						<item>correct</item>
					</one-of>
				</item>
			</one-of>
		<tag>BOOLEAN=”true”</tag></item> 
		<item>
			<one-of>
				<item>no</item>
				<item>nope</item>
			</one-of>
		<tag>BOOLEAN=”false”</tag></item>
	</one-of>
	</rule>
	<rule id="Adverb_Entirely">
		<one-of>
			<item>really</item>
			<item>perfectly</item>
			<item>absolutely</item>
			<item>totally</item>
			<item>all</item>
			<item>entirely</item>
			<item>completely</item>
			<item>fully</item>
			<item>exactly</item>
		</one-of>
	 </rule>
	</grammar>
	
	
