# UCD

UML Class Diagram (UCD) is a language for specifying UML class diagrams and a tool for converting it into various
different formats.

## Install

### Bundler: `gem "ucd"`

### RubyGems: `gem install ucd`

## Executable

```sh
# Convert composite.ucd to composite.png
$ ucd --type png --output . composite.ucd

# Display detailed help message
$ ucd --help
```

## Language

### Examples

See the examples directory for UCD files and their generated DOT and PNG files.

#### Component Pattern

`component.ucd`

```ruby
abstract class Component {
  method operation
}

class Leaf {
  generalizes Component
}

class Composite {
  protected field children : Component
  method addChild(child : Component) : Component
  method removeChild(child : Component) : Component
  method getChild(index : Integer) : Component

  generalizes Component
  aggregation Component *
}
```

`component.png`

![Component Pattern](https://github.com/RyanScottLewis/ucd/raw/master/examples/composite.png)

`component.dot`

```dot
digraph G {
  graph [splines="ortho" rankdir="BT"]
  edge [color="gray50"]
  node [shape="plain"]

  ClassComponent [label=<
    <TABLE BORDER="0" CELLBORDER="1" CELLSPACING="0">
      <TR>
        <TD>«abstract»<BR/><I><B>Component</B></I></TD>
      </TR>
      <TR>
        <TD></TD>
      </TR>
      <TR>
        <TD>
          <TABLE BORDER="0" CELLPADDING="0" CELLSPACING="0">
            <TR><TD ALIGN="LEFT">+ operation()</TD></TR>
          </TABLE>
        </TD>
      </TR>
    </TABLE>
  >]

  ClassLeaf [label=<
    <TABLE BORDER="0" CELLBORDER="1" CELLSPACING="0">
      <TR>
        <TD><B>Leaf</B></TD>
      </TR>
      <TR>
        <TD></TD>
      </TR>
      <TR>
        <TD></TD>
      </TR>
    </TABLE>
  >]

  ClassComposite [label=<
    <TABLE BORDER="0" CELLBORDER="1" CELLSPACING="0">
      <TR>
        <TD><B>Composite</B></TD>
      </TR>
      <TR>
        <TD>
          <TABLE BORDER="0" CELLPADDING="0" CELLSPACING="0">
            <TR><TD ALIGN="LEFT"># children : Component</TD></TR>
          </TABLE>
        </TD>
      </TR>
      <TR>
        <TD>
          <TABLE BORDER="0" CELLPADDING="0" CELLSPACING="0">
            <TR><TD ALIGN="LEFT">+ addChild(child : Component) : Component</TD></TR>
            <TR><TD ALIGN="LEFT">+ removeChild(child : Component) : Component</TD></TR>
            <TR><TD ALIGN="LEFT">+ getChild(index : Integer) : Component</TD></TR>
          </TABLE>
        </TD>
      </TR>
    </TABLE>
  >]

  ClassLeaf -> ClassComponent [arrowhead="onormal"]
  ClassComposite -> ClassComponent [arrowhead="onormal"]

  ClassComposite -> ClassComponent [dir="back" arrowtail="odiamond" headlabel="*"]
}
```

### Syntax

> Note: Syntax is represented in [ABNF](https://tools.ietf.org/html/rfc5234) and are omitting spaces for clarity

UCD language simply defines one or more `class` definitions, inside of which the fields, methods, relationships, and
class relationships are defined.

```abnf
NAME       = 1*( ALPHA / DIGIT / "-" / "_" )
TYPE       = NAME
CLASS_BODY = *( ( FIELD / METHOD / RELATIONSHIP / CLASS_RELATIONSHIP ) NEWLINE )
ACCESS     = ( "public" / "protected" / "private")
ARGUMENT   = NAME [ ":" TYPE ]
ARGUMENTS  = ARGUMENT *( "," ARGUMENT )
FROM       = 1*( ALPHA / DIGIT / "-" / "_" / "*" )
TO         = FROM

CLASS              = [ "abstract" / "interface" ] "class" NAME [ "{" CLASS_BODY "}" ]
FIELD              = [ "static" ] [ ACCESS ] field NAME [ ":" TYPE ]
METHOD             = [ "abstract" ] [ "static" ] [ ACCESS ] method NAME [ "(" [ ARGUMENTS ] ")"] [ ":" TYPE ]
RELATIONSHIP       = [ "generalizes" / "realizes" ] NAME
CLASS_RELATIONSHIP = [ "directional" / "bidirectional" ] ( "dependency" / "association" / "aggregation" / "composition" ) NAME [ FROM ] [ TO ]
```

## Development

After checking out the repo, run `bin/setup` to install dependencies. Then, run `rake spec` to run the tests. You can also run `bin/console` for an interactive prompt that will allow you to experiment.

To install this gem onto your local machine, run `bundle exec rake install`. To release a new version, update the version number in `version.rb`, and then run `bundle exec rake release`, which will create a git tag for the version, push git commits and tags, and push the `.gem` file to [rubygems.org](https://rubygems.org).

## Contributing

Bug reports and pull requests are welcome on GitHub at https://github.com/RyanScottLewis/ucd. This project is intended to be a safe, welcoming space for collaboration, and contributors are expected to adhere to the [Contributor Covenant](http://contributor-covenant.org) code of conduct.

## License

The gem is available as open source under the terms of the [MIT License](http://opensource.org/licenses/MIT).
