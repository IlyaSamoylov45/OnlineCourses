DTD's, IDs & IDREFS

	- DTDs (Document type descriptors)
	- IDs
	- IDREFS

"VALID" XML

	- Adheres to basic structural requirements as well-formed XML, but also adheres to
		content specific specification
			- Document type Descriptors (DTD)
			- XML Schema (XSD)

DOCUMENT TYPE DESCRIPTOR (DTD)

	- Grammar like language for specifying elements, attributes, nesting, ordering, # of occurrences
	- Also special attribute types ID and IDREFS
	- IDs and IDREFs are special attributes that allow you to specify pointers

DTD/XCD versus none(well-formed)

	Reasons to use DTD:
		- Programs can assume a structure which makes them simpler because they don't have to be doing a lot of error checking on data
		- CSS/XML
		- Specification for what XML should look like
					ex. data exchange
		- Documentation
		- Benefits of typing

	Reasons not to use DTD:
		- flexibility, ease of exchange
		- DTDs can be messy-irregular
		- With XSDs even more so
		- Benefits of "no typing"
