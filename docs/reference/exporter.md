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
