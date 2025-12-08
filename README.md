<!-- <img src="https://giffiles.alphacoders.com/917/9174.gif" width="100%"/> -->

<!--<h3 align="center">🏆 Github Achievements 🏆</h3>
 <img style="width:100%" src="https://github-profile-trophy.vercel.app/?username=ilmedova&theme=algolia&row=1&column=9"> -->
<!-- <table>
  <tr>
    <td valign="top">
      <a style="text-decoration:none" href="https://leetcode.com/ilmedovamahri/">
        <img style="width:100%" src="https://leetcard.jacoblin.cool/ilmedovamahri?theme=nord&font=Ubuntu&ext=contest"/>
      </a>
    </td>
    <td valign="center" align="center">
     <img width="80%" alt='Cold' src="https://raw.githubusercontent.com/BhavyaCodes/BhavyaCodes/master/.github/cat.gif">
    </td>
  </tr>
</table> -->



<!---
<hr/>
<h3>📜 Certifications </h3>
<div>
    <a align=top href="https://www.hackerrank.com/certificates/c73d843dda76"><img style="width: 49%" src="./certificates/python_basic.png"/></a>
    <a align=top href="https://www.linkedin.com/learning/certificates/1aa641a3ae5e8d5a2c8592c49fa521e9f62fda75d938f6efb77274a6461ccab5"><img style="width: 49%" src="./certificates/python_professional.png"/></a>
</div>
-->
"use client";

import * as React from "react";
import { Check, ChevronsUpDown } from "lucide-react";
import { cn } from "@/lib/utils";
import {
  Popover,
  PopoverContent,
  PopoverTrigger,
} from "@/components/ui/popover";
import { Command,
  CommandEmpty,
  CommandGroup,
  CommandInput,
  CommandItem,
  CommandList
} from "@/components/ui/command";

// ---------------------------------------------------------------------------
// TYPES
// ---------------------------------------------------------------------------
export type MultiSelectOption = {
  label: string;
  value: string;
};

interface MultiSelectProps {
  options: MultiSelectOption[];
  value: string[];
  onValueChange: (vals: string[]) => void;
  placeholder?: string;
  className?: string;
}

// ---------------------------------------------------------------------------
// MULTISELECT COMPONENT — looks exactly like shadcn <Select>
// ---------------------------------------------------------------------------
export function MultiSelect({
  options,
  value,
  onValueChange,
  placeholder = "Select options",
  className,
}: MultiSelectProps) {
  const [open, setOpen] = React.useState(false);

  function toggle(val: string) {
    if (value.includes(val)) {
      onValueChange(value.filter((v) => v !== val));
    } else {
      onValueChange([...value, val]);
    }
  }

  const selectedLabels = value
    .map((v) => options.find((o) => o.value === v)?.label)
    .filter(Boolean)
    .join(", ");

  return (
    <Popover open={open} onOpenChange={setOpen}>
      <PopoverTrigger
        asChild
        className={cn("w-full", className)}
      >
        <button
          type="button"
          // Same styles as shadcn Select trigger
          className={cn(
            "flex h-10 w-full items-center justify-between rounded-md border border-input bg-background px-3 py-2 text-sm",
            "ring-offset-background placeholder:text-muted-foreground",
            "focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-ring focus-visible:ring-offset-2",
            "disabled:cursor-not-allowed disabled:opacity-50"
          )}
        >
          {selectedLabels || (
            <span className="text-muted-foreground">{placeholder}</span>
          )}
          <ChevronsUpDown className="h-4 w-4 opacity-50" />
        </button>
      </PopoverTrigger>

      <PopoverContent className="w-[200px] p-0">
        <Command>

          <CommandInput placeholder="Search..." />

          <CommandList>
            <CommandEmpty>No results.</CommandEmpty>

            <CommandGroup>
              {options.map((opt) => {
                const isSelected = value.includes(opt.value);

                return (
                  <CommandItem
                    key={opt.value}
                    value={opt.label}
                    onSelect={() => toggle(opt.value)}
                    className="cursor-pointer"
                  >
                    <div
                      className={cn(
                        "mr-2 flex h-4 w-4 items-center justify-center rounded-sm border",
                        isSelected
                          ? "bg-primary text-primary-foreground"
                          : "opacity-50"
                      )}
                    >
                      {isSelected && <Check className="h-3 w-3" />}
                    </div>

                    {opt.label}
                  </CommandItem>
                );
              })}
            </CommandGroup>
          </CommandList>
        </Command>
      </PopoverContent>
    </Popover>
  );
}
