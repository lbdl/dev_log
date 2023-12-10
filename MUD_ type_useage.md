## custom types array's in `valueSchema`
adding user types as dynamic arrays as values for later tables using the types name fails
e.g.

```typescript
Room: {
            keySchema: {
                roomId: "uint32",
            },
            valueSchema: {
                textDefId: "uint32",
                actions: "Action[]",
                roomType: "RoomType", 
            },
        }
```
throws a compilation error:

```javascript
ValidationError [ZodValidationError]: StoreConfig Validation Error

- Action[] is not a valid abi type, and is not defined in userTypes
 ...
 ... <log chopped here>
 ...
  details: [
    {
      code: 'custom',
      message: 'Action[] is not a valid abi type, and is not defined in userTypes',
      path: []
    }
  ]
 ```

the reason is dscribed under the seciton about [encoding](https://mud.dev/store/encoding).

MUD `tables` have 28 possible fields usable as a `valueType` of 5 can be of a `dynamic` size, however this dynamic fields does not understand `type` in the sense other than an ETH primitive. So the value of the  field has to be `bytes`.

Further `dynamic` fields have to be declared after all fixed length fields declarations.

This then is correct:
```typescript
Room: {
            keySchema: {
                roomId: "uint32",
            },
            valueSchema: {
                textDefId: "uint32",
                actions: "Action[]",
                roomType: "RoomType", 
            },
        }
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTUxODMxNzMyNF19
-->