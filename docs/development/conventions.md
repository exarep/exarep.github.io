# Conventions

Coding conventions and standards followed across all Exarep repositories.

## Java Conventions

### Dependency Injection

Always use constructor injection. Do not use `@Inject`.

```java
private final CustomerRepository customerRepository;

public CustomerService(CustomerRepository customerRepository) {
    this.customerRepository = customerRepository;
}
```

### Records as DTOs

Use Java records for all data transfer objects, domain objects, and value objects:

```java
public record CustomerResponse(
    Long customerId,
    String firstName,
    String lastName,
    String email
) {}
```

### Nullable Return Types

Use `Optional` for any method that may return null:

```java
public Optional<Customer> findByCustomerId(Long customerId) {
    return customerRepository.findByCustomerId(customerId)
        .map(this::toDomain);
}
```

### Roles

Define roles as constants in an interface:

```java
public interface Roles {
    String CUSTOMER_READ = "customer-read";
    String CUSTOMER_WRITE = "customer-write";
    String CUSTOMER_ADMIN = "customer-admin";
}
```

Apply at the method level:

```java
@GET
@Path("/{customerId}")
@RolesAllowed(Roles.CUSTOMER_READ)
public Response getCustomer(@PathParam("customerId") Long customerId) {
    // ...
}
```

### Validation

Use Hibernate Validator annotations with messages from `ValidationMessages.properties`:

```java
@NotBlank(message = "{customer.firstName.required}")
@Column(name = "first_name", nullable = false)
public String firstName;
```

## JPA Entity Conventions

- Class name suffixed with `Entity` (e.g., `CustomerEntity`)
- `@Entity(name = "Customer")` and `@Table(name = "customer")`
- ID follows `{entity}Id` pattern (e.g., `customerId`)
- All fields are `public`
- Include `@Column` with `name` attribute on every field
- Include `hashCode`, `equals`, and `toString`

```java
@Entity(name = "Customer")
@Table(name = "customer")
public class CustomerEntity extends PanacheEntityBase {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @Column(name = "customer_id")
    public Long customerId;

    @NotBlank(message = "{customer.firstName.required}")
    @Column(name = "first_name", nullable = false)
    public String firstName;

    @NotBlank(message = "{customer.lastName.required}")
    @Column(name = "last_name", nullable = false)
    public String lastName;
}
```

## REST API Conventions

- URL versioning: `/api/v1/{resource}`
- Return `jakarta.ws.rs.core.Response` from all endpoint methods
- Use `@APIResponse` annotations for OpenAPI documentation
- `@RolesAllowed` at the method level
- Resource classes interact only with the service layer

## Database Conventions

- Singular table names (`customer`, not `customers`)
- Exception: `users` table (reserved word)
- `BIGSERIAL` / `BIGINT` for primary and foreign keys — no UUIDs
- `TEXT` for alphanumeric data — no `VARCHAR`
- Include `created_at` and `updated_at` timestamps
- Set sequence starting values: `ALTER SEQUENCE customer_customer_id_seq RESTART 1000000000;`

## Angular Conventions

- Use signals and `inject()` instead of constructor injection
- Page components in `src/app/pages/` with `.page.` suffix
- Shared components in `src/app/shared/` with `.component.` suffix
- Services in `src/app/services/` with `.service.ts` suffix
- Guards in `src/app/guards/` with `.guard.ts` suffix
- Use ng-bootstrap components
- Entity IDs prefixed with domain name (e.g., `accountId`, not `id`)

## Container Conventions

- Use `Containerfile`, not `Dockerfile`
- Assume Podman as the container runtime
- Source images from Red Hat registries (`registry.access.redhat.com`, `registry.redhat.io`, `quay.io`)

## Git Conventions

### Repository Naming

| Prefix          | Purpose                                   |
| --------------- | ----------------------------------------- |
| `api-`          | Deployable backend services               |
| `portal-`       | Authenticated web applications            |
| `web-`          | Marketing sites, docs, mobile shells      |
| `gitops-`       | OpenShift GitOps (Argo CD, Kustomize)     |
| `iac-`          | Infrastructure automation (Terraform, Ansible) |
| `library-`      | Shared libraries                          |
| `integration-`  | Batch or cron scheduled jobs              |

## Protobuf and gRPC Conventions

- Use proto3 syntax
- Enum values prefixed with uppercase snake case of the enum name
- Always include `UNSPECIFIED` as the 0 value

```protobuf
enum CustomerType {
  CUSTOMER_TYPE_UNSPECIFIED = 0;
  CUSTOMER_TYPE_RESIDENTIAL = 1;
  CUSTOMER_TYPE_COMMERCIAL = 2;
}
```
