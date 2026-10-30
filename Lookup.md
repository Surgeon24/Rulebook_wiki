# Lookup

## Summary

Retrieves a value from the column specified by the [MatrixCol](elements/MatrixCol.md) attribute or child rule in a [Matrix](elements/Matrix.md) whose definition is specified by the [MatrixDef](elements/MatrixDef.md) attribute or, from v2.9, child rule, based upon the keys supplied.

The matrix name can be read  from the results of the rules within the [Matrix](elements/Matrix.md) function, or from the [MatrixFromConstant](attributes/Lookup/MatrixFromConstant.md), [MatrixFromDataDef](attributes/Lookup/MatrixFromDataDef.md), [MatrixFromInput](attributes/Lookup/MatrixFromInput.md) or [MatrixFromParam](attributes/Lookup/MatrixFromParam.md) attributes. The keys can be obtained from rules within [Keys](elements/Keys.md) function, or from a single key supplied by [KeyFromConstant](attributes/Lookup/KeyFromConstant.md), [KeyFromDataDef](attributes/Lookup/KeyFromDataDef.md), [KeyFromInput](attributes/Lookup/KeyFromInput.md), or [KeyFromParam](attributes/Lookup/KeyFromParam.md). When a direct key-source attribute is used, [KeyFromType](attributes/Lookup/KeyFromType.md) must also be specified.

## Contents

- [Attributes](#available-attributes)
- [Child Rules](#child-rules)
- [Example of usages](#example-of-usages)
- [Remarks](#remarks)
- [Version History](#version-history)

## Available Attributes

![Lookup example](../assets/lookup_1.png)
The table covers all available attributes for this element.

| Attribute | Purpose | Requirement / alternative | Default value |
|---|---|---|---|
| [Type](attributes/Lookup/Type.md) | Return datatype | Required, must match the selected column type specified in MatrixDef. | - |
| [MatrixDef](elements/MatrixDef.md) | Definition of the matrix being queried | Attribute, or child rule from v2.9 | - |
| [MatrixCol](elements/MatrixCol.md) | Column from which to retrieve values | Attribute or child rule | - |
| [MatrixFromConstant](attributes/Lookup/MatrixFromConstant.md) | [Matrix](elements/Matrix.md) name supplied as a constant | Exactly one of the "MatrixFrom..." options is mandatory | - |
| [MatrixFromDataDef](attributes/Lookup/MatrixFromDataDef.md) | [Matrix](elements/Matrix.md) name supplied from data-definition data | Exactly one of the "MatrixFrom..." options is mandatory | - |
| [MatrixFromInput](attributes/Lookup/MatrixFromInput.md) | [Matrix](elements/Matrix.md) name supplied from input | Exactly one of the "MatrixFrom..." options is mandatory | - |
| [MatrixFromParam](attributes/Lookup/MatrixFromParam.md) | [Matrix](elements/Matrix.md) name supplied from a parameter | Exactly one of the "MatrixFrom..." options is mandatory | - |
| [KeyFromConstant](attributes/Lookup/KeyFromConstant.md) | Single key supplied as a constant | Alternative to [Keys](elements/Keys.md) and other "KeyFrom..." attributes. Requires [KeyFromType](attributes/Lookup/KeyFromType.md). | - |
| [KeyFromDataDef](attributes/Lookup/KeyFromDataDef.md) | Single key supplied from data-definition data | Alternative to [Keys](elements/Keys.md) and other "KeyFrom..." attributes. Requires [KeyFromType](attributes/Lookup/KeyFromType.md). | - |
| [KeyFromInput](attributes/Lookup/KeyFromInput.md) | Single key supplied from input | Alternative to [Keys](elements/Keys.md) and other "KeyFrom..." attributes. Requires [KeyFromType](attributes/Lookup/KeyFromType.md). | - |
| [KeyFromParam](attributes/Lookup/KeyFromParam.md) | Single key supplied from a parameter | Alternative to [Keys](elements/Keys.md) and other "KeyFrom..." attributes. Requires [KeyFromType](attributes/Lookup/KeyFromType.md). | - |
| [KeyFromType](attributes/Lookup/KeyFromType.md) | Datatype of a directly supplied key | Required when a direct key-source attribute is used. Must match the corresponding key column type | - |
| [ResultMode](attributes/Lookup/ResultMode.md) | Determines permitted result cardinality | Optional | FirstResult |
| [ValidateHasAllKeys](attributes/Lookup/ValidateHasAllKeys.md) | Checks that at least one key rule or key attribute is supplied | Optional | true |

## Child Rules

| Child rule | Purpose | Notes |
|---|---|---|
| [Matrix](elements/Matrix.md) | Evaluates the matrix name | Alternative to matrix-source attributes |
| [MatrixDef](elements/MatrixDef.md) | Evaluates the matrix definition | Available as a child rule from v2.9 |
| [MatrixCol](elements/MatrixCol.md) | Evaluates the target column | Alternative to the column attribute |
| [Keys](elements/Keys.md) | Contains expressions providing key values | Unnamed expressions map to matrix keys by definition order |
| [Key](elements/Key.md), within [Keys](elements/Keys.md) | Provides a value for a named key column | Allows a later key to be supplied without preceding keys |

### Attributes on the Named-[Key](elements/Key.md) Child Rule

![Lookup example](../assets/lookup_2.png)
These attributes belong to [Key](elements/Key.md), not directly to Lookup.

| Attribute | Purpose | Requirement / default | Version notes |
|---|---|---|---|
| [KeyName](attributes/Key/KeyName.md) | Identifies the matrix key column to match | Required in the named-key form | Named keys available from v2.0 |
| [IgnoreOnNullValue](attributes/Key/IgnoreOnNullValue.md) | Omits this key when its child expression returns null | Optional; default not specified in the supplied article | Available from v2.2 |

### Positional [Keys](elements/Keys.md)

By default, child expression results inside [Keys](elements/Keys.md) are applied in matrix key-definition order. If the matrix defines KeyA followed by KeyB, the first expression matches KeyA and the second matches KeyB.

### Named [Keys](elements/Keys.md)

From v2.0, named keys allow the second or a subsequent key column to be specified without supplying the first. Each [Key](elements/Key.md) contains a child expression returning the value used for matching. [KeyName](attributes/Key/KeyName.md) identifies the column.

### Ignore On Null Value

As of v2.2 there is an additional property available on the Key rule, IgnoreOnNullValue. This can be used to specify that the key will not be used in the lookup if the result of the child rule is null. In the following example the key value for KeyA is null so only KeyB will used in the lookup:

```xml
<Lookup>
  <Keys>
    <Key KeyName="KeyA" IgnoreOnNullValue="true">
      <Null />
    </Key>
    <Key KeyName="KeyB">
      <Constant Type="string">ABC</Constant>
    </Key>
  </Keys>
</Lookup>
```

## Example of usages

### 1. One [Key](elements/Key.md) in a Table with One [Key](elements/Key.md)

**Setup:** a [Matrix](elements/Matrix.md) named Trades using a [MatrixDef](elements/MatrixDef.md) named TradesDef. TradeCode is the only key column, with integer values; Rate is a decimal value column.

| TradeCode | Rate |
|---|---|
| 1001 | 0.75 |
| 1002 | 1.10 |

```xml
<Lookup Type="decimal"
        MatrixDef="TradesDef"
        MatrixFromConstant="Trades"
        MatrixCol="Rate"
        KeyFromConstant="1001"
        KeyFromType="integer"
        ResultMode="SingleResult" />
```

**Expected result for this fixture:** 0.75. The directly supplied key matches TradeCode. SingleResult requires exactly one matching result.

### 2. Two [Keys](elements/Keys.md) in a Table with Two [Keys](elements/Keys.md)

**Setup:** a [Matrix](elements/Matrix.md) named Trades using a [MatrixDef](elements/MatrixDef.md) named TradesDef. The key columns are TradeCode and CoverageType, integer and string respectively; Rate is a decimal value column.

| TradeCode | CoverageType | Rate |
|---|---|---|
| 1001 | EL | 0.75 |
| 1001 | PL | 0.40 |
| 1002 | EL | 1.10 |

```xml
<Lookup Type="decimal"
        MatrixDef="TradesDef"
        MatrixFromConstant="Trades"
        MatrixCol="Rate"
        ResultMode="SingleResult">
  <Keys>
    <Key KeyName="TradeCode">
      <Constant Type="integer">1001</Constant>
    </Key>
    <Key KeyName="CoverageType">
      <Constant Type="string">EL</Constant>
    </Key>
  </Keys>
</Lookup>
```

**Expected result for this fixture:** 0.75. Both named columns are matched. The combination selects only one row.

### 3. One [Key](elements/Key.md) in a Table with Two [Keys](elements/Keys.md)

**Setup:** a [Matrix](elements/Matrix.md) named Trades using a [MatrixDef](elements/MatrixDef.md) named TradesDef. TradeCode is the first integer key; CoverageType is the second string key. Rate is a decimal value column.

| TradeCode | CoverageType | Rate |
|---|---|---|
| 1001 | EL | 0.75 |
| 1001 | PL | 0.40 |
| 1002 | EL | 1.10 |

```xml
<Lookup Type="decimal"
        MatrixDef="TradesDef"
        MatrixFromConstant="Trades"
        MatrixCol="Rate"
        ResultMode="MultipleResult">
  <Keys>
    <Key KeyName="CoverageType">
      <Constant Type="string">EL</Constant>
    </Key>
  </Keys>
</Lookup>
```

**Expected matching values for this fixture:** 0.75 and 1.10, with no ordering guarantee asserted. Only CoverageType is supplied; TradeCode is not constrained. Named keys explicitly support supplying a later column without earlier columns. MultipleResult permits zero or more results.

**Why use a named key?** An unnamed expression inside [Keys](elements/Keys.md) would match the first defined key, TradeCode, rather than the intended second key, CoverageType. [KeyName](attributes/Key/KeyName.md) selects the intended column.

**Why not disable validation?** A key is already supplied. [ValidateHasAllKeys](attributes/Lookup/ValidateHasAllKeys.md) currently checks for at least one key, not every key. Its default can therefore be retained for this documented scenario.

**Cardinality alternatives:** SingleResult would fail for the two matching rows in this fixture. FirstResult would return only the first result found. MultipleResult retains the matching values.

### 4. <Matrix> Child Rule

Instead of "MatrixFrom..." we can supply Matrix Name with <Matrix> child Rule.
```xml
<Lookup Type="decimal"
        MatrixDef="TradesDef"
        MatrixCol="Rate"
        ResultMode="SingleResult"
        KeyFromConstant="1001"
        KeyFromType="integer">
  <Matrix>
        <FirstValue Type="string" FromConstant="" FromDataDef="/Scheme/LookupData/@Trades" Order="DataDefInputParamConstant"/>
  </Matrix>
</Lookup>
```

### 5. <MatrixCol> Child Rule

Instead of "MatrixCol" we can define Matrix Column under <MatrixCol> child Rule.
```xml
<Lookup Type="decimal"
        MatrixDef="TradesDef"
        MatrixDef="/Scheme/LookupData/@Trades"
        ResultMode="SingleResult"
        KeyFromConstant="1001"
        KeyFromType="integer">
  <MatrixCol>
        <FirstValue Type="string" FromConstant="" FromInput="ELTab/ColumnName" Order="DataDefInputParamConstant"/>
  </MatrixCol>
</Lookup>
```

## Remarks

### Result Cardinality

[ResultMode](attributes/Lookup/ResultMode.md) determines how many results are allowed.

| Mode | No matches | One match | More than one match |
|---|---|---|---|
| SingleResult | Error | Returns the result | Error |
| FirstResult | No result | Returns the result | Returns the first result found |
| MultipleResult | Zero results | Matching result | Matching results |

The default is FirstResult.

### [Key](elements/Key.md) Validation

[ValidateHasAllKeys](attributes/Lookup/ValidateHasAllKeys.md) is currently used only to ensure that at least one key rule or attribute is specified when enabled. Its default is true. Do not interpret the attribute name as requiring all matrix keys.

### Performance

A lookup is faster if:
* It searches on all the keys,
* There are no range columns,
* The ResultMode=”FirstResult”
* It is looking for a value column.

## Version History

| RuleBook version | Change described in the supplied article |
|---|---|
| v2.0 | Named [Key](elements/Key.md) rules allow later key columns to be supplied without preceding columns |
| v2.2 | [IgnoreOnNullValue](attributes/Key/IgnoreOnNullValue.md) added to [Key](elements/Key.md) |
| v2.9 | [MatrixDef](elements/MatrixDef.md) can be supplied through a child rule instead of only an attribute |
