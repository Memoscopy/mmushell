## Exporter

::: mmushell.exporter

::: mmushell.exporter._explore_radixtree
# Understanding Memory Mapping Function: _explore_radixtree

## Objectives

- Explore a radix tree to obtain virtual <-> physical mappings.

## Functioning

- Exploring a radix tree involves a recursive approach, which is what `_explore_radixtree` does to traverse it in our case.

## Mapping Construction

- Virtual <-> physical mappings are constructed by analyzing each entry of the radix tree table.
- In our case, they are built at each level of the tree, associating computed virtual addresses with corresponding physical addresses `mapping[permissions].append((virt_addr, page_size, phy_addr, in_mmd))`.

::: mmushell.exporter._reconstruct_mappings
# Understanding Memory Mapping Function: _reconstruct_mappings

## Exploring Radix Tree

- Initiates mappings and calls `_explore_radixtree()` with necessary parameters.
- Populates `mapping` and `reverse_mapping` with mappings obtained from the radix tree.

## ELF Virtual Mapping Reconstruction

- Sets `reverse_mapping` and `mapping` attributes for potential future reference.
- Likely essential for reconstructing ELF (Executable and Linkable Format) virtual mappings.

## Collecting Intervals

- Collects intervals based on mappings, excluding user-inaccessible pages.
- Compiles intervals with start address, end address, physical page, and permission mask.

## Interval Fusion

- Performs operations to compact intervals for efficiency.
- Reduces the number of elements for faster processing.

## Offset to Virtual Mapping

- Translates physical offsets to virtual addresses.
- Creates intervals for physical offsets to virtual addresses based on `reverse_mapping`.

## Sorting Intervals

- Sorts the intervals obtained from the previous steps.
- Essential for organized and efficient processing.

## Creating Resolution Objects

- Constructs resolution objects using collected and sorted interval data.
- Includes objects such as `IMOffsets`, `IMOverlapping`, `IMData`.

::: mmushell.exporter._export_virtual_memory_elf

# Understanding Function: _export_virtual_memory_elf

## Function Name: _export_virtual_memory_elf

### Arguments:

- `self`: The instance of the class invoking the function.
- `elf_filename`: Name of the file to create.

### Expected Results:

Creation of an ELF file.

### Purpose:

To create an ELF file containing the virtual address space of the process.

## Functioning:

- The ELF header is created based on the characteristics of the target machine.
- Machine data is retrieved from `self.phy.get_machine_data()`.
- Endianness and architecture are extracted from this data.
- Depending on the architecture of the machine, the value of `e_machine` is determined.
- Various parts of the header are filled with appropriate values, such as the magic number, ELF type, endianness, etc.

- The virtual mappings of the process are processed to group them into memory segments in the ELF file.
- The virtual mappings of the process are traversed from the dictionary `self.mapping`.
- For each `pmask` (bitmask indicating permissions), associated memory intervals are retrieved and extended in the `intervals` dictionary.
- Memory intervals are compacted to reduce the number of memory segments. This is done by merging adjacent intervals with the same physical offset.
- The compact intervals are then sorted based on their decreasing size.

- A program header is created for each memory segment, specifying the virtual, physical address, and size of the segment.
- For each segment, details such as virtual address, physical address, and size are calculated.
- The data is written to the resulting ELF file.

- The ELF header is modified to point to the program header, ensuring the consistency and validity of the ELF files.
- If the number of segments is less than 65536, the ELF header is modified to reflect this number.
- Otherwise, adjustments are made to handle the high number of segments in accordance with ELF64 specifications.
