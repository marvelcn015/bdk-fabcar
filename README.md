# Fabcar Chaincode - Golang Implementation

A comprehensive smart contract (chaincode) for managing car assets on Hyperledger Fabric blockchain network.

## Overview

This chaincode implements a car management system with full CRUD operations, validation, and querying capabilities. It demonstrates best practices for Hyperledger Fabric chaincode development including proper error handling, data validation, and structured code organization.

## Features

### Core Operations
- **Create Car**: Add new car records to the blockchain
- **Query Car**: Retrieve specific car information by key
- **Update Car**: Modify existing car attributes
- **Delete Car**: Remove car records from the blockchain
- **Change Owner**: Transfer car ownership

### Advanced Queries
- **Query All Cars**: Retrieve all car records
- **Query by Owner**: Find all cars owned by a specific person
- **Query by Range**: Retrieve cars within a key range
- **Get Car History**: View complete transaction history for a car
- **Get Car Count**: Count total number of cars in the ledger

### Data Validation
- Comprehensive input validation for all car attributes
- Year validation (1900-2030)
- Price validation (non-negative values)
- String length limits and format validation
- Owner name format validation

## Data Model

```go
type Car struct {
    ObjectType string `json:"docType"`
    Make       string `json:"make"`
    Model      string `json:"model"`
    Color      string `json:"color"`
    Owner      string `json:"owner"`
    Year       int    `json:"year"`
    Price      int    `json:"price"`
}
```

## API Methods

### 1. Initialize Chaincode
```
Function: Init
Description: Initializes the chaincode with sample car data
```

### 2. Query Single Car
```
Function: queryCar
Arguments: [carKey]
Example: queryCar "CAR0"
```

### 3. Query All Cars
```
Function: queryAllCars
Arguments: []
Example: queryAllCars
```

### 4. Create New Car
```
Function: createCar
Arguments: [carKey, make, model, color, owner, year, price]
Example: createCar "CAR100" "BMW" "X5" "black" "John Doe" "2022" "75000"
```

### 5. Update Car
```
Function: updateCar
Arguments: [carKey, make, model, color, owner, year, price]
Example: updateCar "CAR0" "Toyota" "Prius" "green" "Tomoko" "2018" "26000"
```

### 6. Change Car Owner
```
Function: changeCarOwner
Arguments: [carKey, newOwner]
Example: changeCarOwner "CAR0" "Alice Smith"
```

### 7. Delete Car
```
Function: deleteCar
Arguments: [carKey]
Example: deleteCar "CAR0"
```

### 8. Query Cars by Owner
```
Function: queryCarsByOwner
Arguments: [ownerName]
Example: queryCarsByOwner "John Doe"
```

### 9. Query Cars by Range
```
Function: queryCarsByRange
Arguments: [startKey, endKey]
Example: queryCarsByRange "CAR0" "CAR9"
```

### 10. Get Car History
```
Function: getCarHistory
Arguments: [carKey]
Example: getCarHistory "CAR0"
```

### 11. Get Car Count
```
Function: getCarCount
Arguments: []
Example: getCarCount
```

## File Structure

```
go/
├── go.mod              # Go module dependencies
├── main.go             # Chaincode entry point
├── fabcar.go           # Main chaincode structure and interface
├── business_logic.go   # Core business operations (CRUD)
├── queries.go          # Advanced query operations
├── utils.go            # Validation and utility functions
├── fabcar_test.go      # Comprehensive test suite
└── README.md           # This documentation
```

## Installation and Deployment

### Prerequisites
- Go 1.21 or higher
- Hyperledger Fabric 2.x
- Docker and Docker Compose

### Build
```bash
cd /path/to/chaincode/go
go mod tidy
go build
```

### Test
```bash
go test -v
```

### Package for Deployment
```bash
# From your Fabric network directory
peer lifecycle chaincode package fabcar.tar.gz --path ./chaincode/fabcar/go --lang golang --label fabcar_1.0
```

## Error Handling

The chaincode implements comprehensive error handling:
- Input validation errors
- Business logic errors
- Blockchain state errors
- JSON marshaling/unmarshaling errors

All errors are returned with descriptive messages to help with debugging and user experience.

## Security Considerations

- All input parameters are validated before processing
- SQL injection prevention through parameterized queries
- String length limits to prevent buffer overflow attacks
- Type validation for numeric inputs
- Sanitization of user input data

## Testing

The test suite covers:
- Chaincode initialization
- All CRUD operations
- Error scenarios and edge cases
- Data validation functions
- Utility function validation

Run tests with:
```bash
go test -v -cover
```

## CouchDB Queries

Some operations (like `queryCarsByOwner` and `getCarCount`) require CouchDB as the state database for rich queries. These operations will not work with the default LevelDB.

## Performance Considerations

- Efficient key-based lookups for single car queries
- Pagination support for large result sets (can be added)
- Minimal state reads/writes for optimal performance
- Indexed queries when using CouchDB

## Future Enhancements

Potential improvements:
- Pagination for large query results
- Advanced search filters (price range, year range, etc.)
- Car maintenance records
- Multi-signature ownership transfers
- Event emission for real-time notifications
- Encryption for sensitive data

## Support

For issues and questions:
- Check the test suite for usage examples
- Review error messages for troubleshooting
- Refer to Hyperledger Fabric documentation for deployment guidance