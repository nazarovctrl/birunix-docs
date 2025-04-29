# Runtime Services

### Overview

The `RuntimeService` is a component of the Biruni framework designed to handle tasks that are not efficiently performed in Oracle PL/SQL, such as sending HTTP requests, performing complex computations, or integrating with external systems. It is invoked during request processing, executes in a Java/Scala environment, and returns results to a specified PL/SQL response procedure.

### Usage

#### Implementing a Runtime Service

To create a custom `RuntimeService`, extend the `uz.greenwhite.biruni.old.service.RuntimeService` class and override the `run(detail: Map[String, Any], data: String): RuntimeResult` method.

The `run` method processes input data, where:

* `detail`: A map containing metadata about the request (e.g., company ID, instance code).
* `data`: A string (typically JSON) containing the payload to process.

The method returns a `RuntimeResult`, which can be either:

* `SuccessRuntimeResult`: Contains the successful result (e.g., a JSON string).
* `ErrorRuntimeResult`: Contains an error message if an exception occurs.

#### **Steps:**

1. **Create a Service Class**: Annotate your class with `@Service` and extend `RuntimeService`.
2. **Parse Input Data**: Parse the `data` string (e.g., JSON) and use `detail` for context.
3. **Implement Logic**: Process the data in the `run` method and return a `RuntimeResult`.

#### **Example**

The following example implements a `BiruniCryptoService` that encrypts a secret key using RSA encryption.

```scala
import org.springframework.stereotype.Service
import uz.greenwhite.biruni.old.service.RuntimeService
import uz.greenwhite.biruni.old.service.RuntimeResult
import uz.greenwhite.biruni.util.JSON

@Service
class BiruniCryptoService extends RuntimeService {

  override def run(detail: Map[String, Any], data: String): RuntimeResult = {
    try {
      var m: Map[String, Any] = JSON.parseForce(data)
      m += "secret_key" -> DigitalSignatureRSA.encrypt(
        m("secret_key").asInstanceOf[String],
        m("rsa_public_key").asInstanceOf[String]
      )
      SuccessRuntimeResult(JSON.stringify(m))
    } catch {
      case ex: Exception => ErrorRuntimeResult(ex.getMessage)
    }
  }
}
```

#### Invoking Runtime Service from PL/SQL

To trigger a `RuntimeService`, create a `Runtime_Service` instance in PL/SQL, configure it with details, data, and procedures, and queue it for execution.

**Example**

The following PL/SQL code creates and configures a `RuntimeService` for `BillingIntegrationService`:

```sql
DECLARE
  v_Service Runtime_Service;
  v_Details Hashmap := Hashmap();
  v_Data VARCHAR2(4000) := '{"key":"value"}';
BEGIN
  -- Initialize the RuntimeService with the class name
  v_Service := Runtime_Service('uz.greenwhite.biruni.old.service.runtimeservice.BillingIntegrationService');

  -- Set metadata (detail)
  v_Details.Put('company_id', '123');
  v_Details.Put('instance_code', 'INST_001');
  v_Service.Set_Detail(v_Details);

  -- Set data (e.g., JSON payload)
  v_Service.Set_Data(v_Data);

  -- Set review procedure (optional)
  v_Service.Set_Review_Procedure('Kl_Core.Billing_Review_Procedure');

  -- Set response procedure and action modes
  v_Service.Set_Response_Procedure(
    Response_Procedure => 'Ui_Biruni177.Response',
    Action_In         => 'M',
    Action_Out        => 'M'
  );
END;
```

* **Key Methods**:
  * **Runtime\_Service(i\_Class\_Name)**: Initializes the service with the fully qualified class name (e.g., `uz.greenwhite.biruni.old.service.runtimeservice.BillingIntegrationService`).
  * **Set\_Detail(i\_Details)**: Sets metadata as a `Hashmap` passed to the `detail` parameter of the `run` method.
  * **Set\_Data(i\_Data)**: Sets the payload (e.g., JSON string) passed to the `data` parameter of the `run` method.
  * **Set\_Review\_Procedure(i\_Procedure)**: Specifies an optional PL/SQL procedure to review the service execution (e.g., `Kl_Core.Billing_Review_Procedure`).
  * **Set\_Response\_Procedure(Response\_Procedure, Action\_In, Action\_Out)**: Specifies the PL/SQL procedure to handle the response, along with input and output action modes.

The response from the `run` method is passed to the specified `Response_Procedure`.

#### Error Handling

If the `run` method encounters an error, it returns an `ErrorRuntimeResult` with the exception message. Implement robust error handling in the `run` method to manage invalid data or external service failures. Errors should be logged and handled within the response procedure or review procedure as configured.

#### Common Use Cases

* **External Requests**: Sending HTTP requests to external APIs.
* **Complex Computations**: Performing tasks like encryption, data transformation, or calculations not suited for PL/SQL.
* **Integration**: Interfacing with external systems or services, such as billing or authentication services.

#### Best Practices

* **Data Parsing**: Safely parse the `data` string (e.g., JSON) to avoid runtime errors. Validate input in the `run` method.
* **Error Handling**: Use `try-catch` blocks to handle exceptions and return `ErrorRuntimeResult` for errors.
* **Testing**: Test your `RuntimeService` with sample `detail` and `data` inputs to ensure correct processing and response handling.
* **Metadata Usage**: Leverage the `detail` map for contextual information (e.g., company ID, instance code) to make the service reusable across different scenarios.
* **Procedure Configuration**: Ensure the review and response procedures are correctly configured to handle the service output or errors.
