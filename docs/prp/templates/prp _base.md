name: "Base PRP Template v2 - TypeScript/Next.js Context-Rich with Validation Loops"
description: |

## Purpose

Template optimized for AI agents to implement TypeScript/Next.js features with sufficient context and self-validation capabilities to achieve working code through iterative refinement.

## Core Principles

1. **Context is King**: Include ALL necessary documentation, examples, and caveats
2. **Validation Loops**: Provide executable tests/lints the AI can run and fix
3. **Information Dense**: Use keywords and patterns from the TypeScript/Next.js codebase
4. **Progressive Success**: Start simple, validate, then enhance
5. **Global rules**: Be sure to follow all rules in CLAUDE.md

---

## Goal

[What needs to be built - be specific about the end state and desires]

## Why

- [Business value and user impact]
- [Integration with existing features]
- [Problems this solves and for whom]

## What

[User-visible behavior and technical requirements]

### Success Criteria

- [ ] [Specific measurable outcomes]

## All Needed Context

### Documentation & References (list all context needed to implement the feature)

```yaml
# MUST READ - Include these in your context window
- url: [Next.js API docs URL]
  why: [Specific sections/methods you'll need]

- file: [path/to/example.tsx]
  why: [Pattern to follow, gotchas to avoid]

- doc: [Library documentation URL]
  section: [Specific section about common pitfalls]
  critical: [Key insight that prevents common errors]

- docfile: [PRPs/ai_docs/file.md]
  why: [docs that the user has pasted in to the project]
```

### Current Codebase tree (run `tree` in the root of the project) to get an overview of the codebase

```bash

```

### Desired Codebase tree with files to be added and responsibility of file

```bash

```

### Known Gotchas of our codebase & Library Quirks

```typescript
// CRITICAL: [Library name] requires [specific setup]
// Example: Next.js requires 'use client' directive for client components
// Example: Server Actions must be async and marked with 'use server'
// Example: We use Zod for runtime validation and type inference
// Example: shadcn/ui components require specific Tailwind config
// Example: Auth.js requires specific callback URLs and session configuration
```

## Implementation Blueprint

### Data models and structure

Create the core data models, ensuring type safety and consistency.

```typescript
Examples:
 - Zod schemas with type inference
 - TypeScript interfaces and types
 - Database schema types (Prisma/Drizzle)
 - API response types
 - Component prop types

```

### list of tasks to be completed to fulfill the PRP in the order they should be completed

```yaml
Task 1:
MODIFY src/existing-component.tsx:
  - FIND pattern: "interface ExistingProps"
  - INJECT after line containing "export interface"
  - PRESERVE existing prop signatures

CREATE src/new-feature.tsx:
  - MIRROR pattern from: src/similar-feature.tsx
  - MODIFY component name and core logic
  - KEEP error handling pattern identical

...(...)

Task N:
...

```

### Per task pseudocode as needed added to each task

```typescript
// Task 1
// Pseudocode with CRITICAL details don't write entire code
async function newFeature(param: string): Promise<Result> {
  // PATTERN: Always validate input first (see lib/validations.ts)
  const validated = featureSchema.parse(param); // throws ZodError

  // GOTCHA: Server Actions need error boundaries
  try {
    // PATTERN: Use existing database connection pattern
    const result = await db.transaction(async (tx) => {
      // CRITICAL: Use optimistic locking for concurrent updates
      const existing = await tx
        .select()
        .from(table)
        .where(eq(table.id, validated.id));

      return await tx.insert(table).values(validated);
    });

    // PATTERN: Revalidate cache after mutations
    revalidatePath("/feature");

    return { success: true, data: result };
  } catch (error) {
    // PATTERN: Standardized error handling
    return { success: false, error: getErrorMessage(error) };
  }
}
```

### Integration Points

```yaml
DATABASE:
  - migration: "Add 'feature_enabled' column to users table"
  - schema: "Update schema.ts with new types"

CONFIG:
  - add to: next.config.js
  - pattern: "Add feature flag environment variables"

ROUTES:
  - add to: app/feature/route.ts
  - pattern: "Create API route with proper error handling"

COMPONENTS:
  - add to: components/ui/
  - pattern: "Follow shadcn/ui component structure"
```

## Validation Loop

### Level 1: Syntax & Style

```bash
# Run these FIRST - fix any errors before proceeding
pnpm run lint                        # ESLint with auto-fix
pnpm run type-check                  # TypeScript compilation check
pnpm run format                      # Prettier formatting

# Expected: No errors. If errors, READ the error and fix.
```

### Level 2: Unit Tests each new feature/file/function use existing test patterns

```typescript
// CREATE __tests__/new-feature.test.tsx with these test cases:
describe("NewFeature", () => {
  it("renders successfully with valid props", () => {
    render(<NewFeature param="valid_input" />);
    expect(screen.getByTestId("feature-content")).toBeInTheDocument();
  });

  it("throws error with invalid input", () => {
    expect(() => featureSchema.parse("")).toThrow();
  });

  it("handles async operations gracefully", async () => {
    const mockFetch = jest.fn().mockRejectedValue(new Error("Network error"));
    global.fetch = mockFetch;

    render(<NewFeature param="valid" />);

    await waitFor(() => {
      expect(screen.getByText(/error/i)).toBeInTheDocument();
    });
  });
});
```

```bash
# Run and iterate until passing:
pnpm run test -- new-feature.test.tsx
# If failing: Read error, understand root cause, fix code, re-run
```

### Level 3: Integration Test

```bash
# Start the development server
pnpm run dev

# Test the page/API endpoint
curl -X POST http://localhost:3000/api/feature \
  -H "Content-Type: application/json" \
  -d '{"param": "test_value"}'

# Expected: {"success": true, "data": {...}}
# If error: Check browser console and terminal for errors

# Test UI component
# Navigate to: http://localhost:3000/feature
# Expected: Component renders without errors
```

### Level 4: Build & Performance

```bash
# Ensure production build works
pnpm run build

# Check bundle size
pnpm run analyze

# Expected: Build succeeds, no significant bundle size increases
```

## Final validation Checklist

- [ ] All tests pass: `pnpm run test`
- [ ] No linting errors: `pnpm run lint`
- [ ] No type errors: `pnpm run type-check`
- [ ] Production build succeeds: `pnpm run build`
- [ ] Manual test successful: [specific URL/interaction]
- [ ] Error boundaries handle failures gracefully
- [ ] Loading states are implemented
- [ ] Accessibility requirements met
- [ ] Performance metrics acceptable

---

## Anti-Patterns to Avoid

- ❌ Don't use `any` type - be explicit with TypeScript types
- ❌ Don't skip 'use client' directive for interactive components
- ❌ Don't ignore Next.js file-based routing conventions
- ❌ Don't fetch data in client components - use Server Components
- ❌ Don't forget to handle loading and error states
- ❌ Don't bypass Zod validation for "simple" cases
- ❌ Don't use inline styles - follow Tailwind/CSS Module patterns
- ❌ Don't ignore accessibility (a11y) requirements
