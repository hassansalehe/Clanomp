# Clanomp
As OpenMP source-to-source translator, Clanomp (pronounced <*c-lan-omp*>) takes your OpenMP program and outputs version of the program without OpenMP pragmas. The OpenMP pragmas are replaced with runtime function calls.

### Goal
The goal is to see how OpenMP program is transformed after OpenMP pragmas are replaced with runtime calls.

### Usage
```bash
~$ clanomp --help
Usage:
  1. $ clanomp <openmp_file>
   e.g. clanomp src/tests/parallel.c

  2. $ clanomp --tests
   to compile all tests in src/tests folder

  3. $ clanomp --help
   to get this help message

```

 ### Sample output
 ```bash
 ~$ clanomp src/tests/single.c 

Translating src/tests/single.c
main
0:   %1 = alloca i32, align 4
0:   %2 = alloca i32, align 4
0:   %3 = alloca %ident_t, align 8
0:   %4 = bitcast %ident_t* %3 to i8*
0:   %5 = bitcast %ident_t* @0 to i8*
0:   call void @llvm.memcpy.p0i8.p0i8.i64(i8* %4, i8* %5, i64 24, i32 8, i1 false)
0:   store i32 0, i32* %1, align 4
60:   int count = 0;
   call void @llvm.dbg.declare(metadata i32* %2, metadata !10, metadata !11), !dbg !12
   store i32 0, i32* %2, align 4, !dbg !12
61:   #pragma omp parallel shared(count)
   %6 = getelementptr inbounds %ident_t, %ident_t* %3, i32 0, i32 4, !dbg !13
   store i8* getelementptr inbounds ([33 x i8], [33 x i8]* @2, i32 0, i32 0), i8** %6, align 8, !dbg !13
   call void (%ident_t*, i32, void (i32*, i32*, ...)*, ...) @__kmpc_fork_call(%ident_t* %3, i32 1, void (i32*, i32*, ...)* bitcast (void (i32*, i32*, i32*)* @.omp_outlined. to void (i32*, i32*, ...)*), i32* %2), !dbg !13
69:   printf("Value of count: %d, construct: <single>\n", count);
   %7 = load i32, i32* %2, align 4, !dbg !14
   %8 = call i32 (i8*, ...) @printf(i8* getelementptr inbounds ([41 x i8], [41 x i8]* @.str.1, i32 0, i32 0), i32 %7), !dbg !15
70:   return 0;
   ret i32 0, !dbg !16
.omp_outlined.
0:   %4 = alloca i32*, align 8
0:   %5 = alloca i32*, align 8
0:   %6 = alloca i32*, align 8
0:   %7 = alloca %ident_t, align 8
0:   %8 = bitcast %ident_t* %7 to i8*
0:   %9 = bitcast %ident_t* @0 to i8*
0:   call void @llvm.memcpy.p0i8.p0i8.i64(i8* %8, i8* %9, i64 24, i32 8, i1 false)
0:   store i32* %0, i32** %4, align 8
0:   call void @llvm.dbg.declare(metadata i32** %4, metadata !24, metadata !11), !dbg !25
0:   store i32* %1, i32** %5, align 8
0:   call void @llvm.dbg.declare(metadata i32** %5, metadata !26, metadata !11), !dbg !25
0:   store i32* %2, i32** %6, align 8
0:   call void @llvm.dbg.declare(metadata i32** %6, metadata !27, metadata !11), !dbg !25
62:   {
   %10 = load i32*, i32** %6, align 8, !dbg !19
63:     #pragma omp single
   %11 = getelementptr inbounds %ident_t, %ident_t* %7, i32 0, i32 4, !dbg !20
   store i8* getelementptr inbounds ([33 x i8], [33 x i8]* @1, i32 0, i32 0), i8** %11, align 8, !dbg !20
   %12 = load i32*, i32** %4, align 8, !dbg !20
   %13 = load i32, i32* %12, align 4, !dbg !20
   %14 = call i32 @__kmpc_single(%ident_t* %7, i32 %13), !dbg !20
   %15 = icmp ne i32 %14, 0, !dbg !20
   br i1 %15, label %16, label %19, !dbg !20
65:       count++;
   %17 = load i32, i32* %10, align 4, !dbg !22
   %18 = add nsw i32 %17, 1, !dbg !22
   store i32 %18, i32* %10, align 4, !dbg !22
66:     }
   call void @__kmpc_end_single(%ident_t* %7, i32 %13), !dbg !25
   br label %19, !dbg !25
63:     #pragma omp single
   %20 = getelementptr inbounds %ident_t, %ident_t* %7, i32 0, i32 4, !dbg !26
   store i8* getelementptr inbounds ([33 x i8], [33 x i8]* @1, i32 0, i32 0), i8** %20, align 8, !dbg !26
   call void @__kmpc_barrier(%ident_t* %7, i32 %13), !dbg !26
67:   }
   ret void, !dbg !27

Value of count: 1, construct: <single>

 ```
