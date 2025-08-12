# DLT-Parser-CANoe_capl
DLT Parser CAPL Script for CANoe (Vector VT System), Parses DLT frames from Ethernet and exposes keyword hits via sysvars for automation/testing.

# DLT Frame Parser & Automation Testing for Vector VT System (CANoe CAPL)

This repository contains CAPL scripts for:
- **DLT Frame Parsing** in CANoe using the Vector VT system.
- **Automation Testing** setup and templates.

---

## How It Works

### Overview

This CAPL script is designed to smartly automate the process of detecting and verifying specific DLT (Diagnostic Log and Trace) messages transmitted via Ethernet in automotive ECUs. It enables rapid automation and validation of logging events for HIL/SIL setups using Vector CANoe and the VT system.

### Workflow

1. **Initialization:**
   - On simulation start or ignition ON (`on start` and `on sysvar IG_ON`), the script resets tracking variables and initializes system variables (sysvars) used to report keyword matches.

2. **Ethernet Monitoring:**
   - The script listens on a user-configured Ethernet port (e.g., `ethernetPort::DLT_PARSING::DIAG_DLT`), which should be set up in CANoe's Vector Hardware Manager to receive DLT frames from the ECU.

3. **Frame Handling:**
   - For each incoming TCP packet from the ECU's DLT source IP and port, the script:
     - Converts the frame payload into both a hexadecimal and an ASCII representation.
     - Maintains a rolling buffer of the last two frames to handle cases where a keyword may be split across two Ethernet frames.

4. **Keyword Matching:**
   - The script retrieves up to two user-specified keywords from sysvars (`Verbose_Keyword1::Value` and `Verbose_Keyword2::Value`).
   - It searches for these keywords in the combined ASCII buffer (covering possible splits between frames).
   - If a keyword is detected, the script:
     - Sets the corresponding `IsAvailable` sysvar to `1`.
     - Stores the detection timestamp in the corresponding `OccurenceTime` sysvar.
     - Optionally logs the detection event to the CANoe output window.

5. **Automation Testing:**
   - The sysvars updated by the parser can be used in further CAPL test scripts or CANoe Test Modules to automate verification of logging behavior.
   - Example automation scripts (see `test/automation_test_script.can`) can be extended to check for keyword occurrences, validate timing, or drive further test logic.

---

## Files

- `DLT_parser_in_CANoe.can`: Main CAPL script for parsing DLT frames received over Ethernet in CANoe.
- `test/automation_test_script.can`: showcase use of DLT frames detected
- `LICENSE`: Open source license (default is MIT).

---

## Getting Started

### Prerequisites

- Vector CANoe (tested with version 16 or later)
- Vector VT System hardware
- Basic familiarity with CAPL scripting

### Usage

1. **Import the CAPL script** to your CANoe simulation.
2. **Configure the `ethernetPort::DLT_PARSING::DIAG_DLT` port** in Vector Hardware Manager to match your DLT Ethernet settings.
3. **Set your DLT keywords** in the respective sysvars (`Verbose_Keyword1::Value`, `Verbose_Keyword2::Value`) in CANoe.
4. **Run the simulation**: DLT messages are parsed and flagged in sysvars. You can use these sysvars in your test automation.

### Automation Testing

- Use the sysvars updated by the parser for automated checks in your test environment.
- Extend the script (see `test/automation_test_script.can`) for specific test cases or result logging.

---

## Contributing

Pull requests and suggestions are welcome! Please open an issue to discuss what you’d like to change.

## License

MIT License. See `LICENSE` for details.
