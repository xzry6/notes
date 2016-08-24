# Encountered Issues
1. 重启后ibus打出来是双拼,设置了也没用.
A: 输入`ibus-daemon –drx`.
2. Maven `OutofMemory: PermGen Space`.
A: `export MAVEN_OPTS="-Xmx512m -XX:MaxPermSize=128m"` then run `mvn install`.
