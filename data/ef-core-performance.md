# Entity Framework Core Performance (Practical Notes)

Entity Framework Core is productive, but performance issues can easily appear
in large or long-lived systems if it is used without discipline.

This document summarizes **practical performance lessons** from enterprise systems.

---

## Common Performance Pitfalls

### 1. Uncontrolled Tracking
By default, EF Core tracks entities, which increases memory usage.

**Guideline**
- Use `AsNoTracking()` for read-only queries
- Enable tracking only when updates are required

---

### 2. N+1 Query Problem
Lazy loading or careless navigation access can generate excessive queries.

**Mitigation**
- Use explicit `Include` / `ThenInclude`
- Prefer projection over loading full graphs
- Log generated SQL during development

---

### 3. Over-fetching Data
Loading full entities when only a subset is required wastes resources.

**Better Approach**
- Use projections (`Select`) to DTOs
- Fetch only required columns
- Avoid returning entity objects from APIs

---

### 4. Inefficient LINQ Expressions
Not all LINQ expressions translate efficiently to SQL.

**Guideline**
- Keep queries simple and database-friendly
- Avoid client-side evaluation
- Review generated SQL for complex queries

---

## Query Optimization Techniques

### Use Compiled Queries (Selective)
For hot paths with repeated queries:
- Use EF Core compiled queries
- Measure before applying globally

---

### Index Awareness
EF Core does not create optimal indexes automatically.

**Best Practices**
- Define indexes explicitly
- Align indexes with query patterns
- Review execution plans regularly

---

## Transactions and Batching

- Group related changes into a single transaction
- Avoid saving changes in loops
- Prefer batch operations where possible

---

## When EF Core Is Not the Right Tool

- Very complex reporting queries
- Heavy batch processing
- Performance-critical analytics

In such cases:
- Use stored procedures
- Use Dapper or raw SQL selectively

This is not a failure of EF Core — it is a pragmatic architectural choice.

---

## Monitoring and Diagnostics

- Enable SQL logging in non-production environments
- Track query execution time
- Monitor memory usage under load
- Profile before optimizing

---

## Key Takeaways

- EF Core is suitable for most business workloads
- Performance issues usually come from misuse, not the framework
- Measure first, optimize second
- Pragmatic mixing of tools often yields the best result

---

## Conclusion

EF Core rewards disciplined usage.

In enterprise systems, **controlled usage and observability**
matter more than micro-optimizations.
